# Ops Scorecard — Report Documentation

**Date:** 2026-03-11
**Analyst:** Maria Jalanie Bette
**Tool:** Tableau
**Audience:** Management team

---

## Purpose

The Ops Scorecard is a Tableau dashboard that tracks KPIs for each operations team. It gives management a consolidated view of team performance across key metrics to support operational oversight and decision-making.

---

## Data Sources

| Data | Source | Notes |
|---|---|---|
| KPI metrics | `dw_gold.fct_agent_daily_metrics` | Gold-layer DBT model; one row per agent per day |
| KPI metrics | `dw_gold.fct_ops_daily_metrics` | Gold-layer DBT model |
| Goals | `employee_meetings.ops_goal_setting` | Accessed via Metabase |

**Note on KPI data:** `dw_gold.fct_agent_daily_metrics` is a gold-layer DBT model. Data at this layer has been cleaned and aggregated from upstream sources. If a KPI value looks off, trace upstream via DBT lineage before assuming the Tableau calc is wrong.

**Note on goals data:** Goals are sourced from Metabase (`employee_meetings.ops_goal_setting`). Cross-check this table in Metabase if goal values appear incorrect in the scorecard.

---

## KPI Definitions

All KPIs are calculated from `dw_gold.fct_agent_daily_metrics`.

| KPI | Formula | What It Measures |
|---|---|---|
| Schedule Adherence | `sum_time_in_adherence_seconds / (sum_time_in_adherence_seconds + sum_time_out_of_adherence_seconds)` | % of time agents were on schedule |
| Utilization % | `sum_actual_working_seconds / sum_paid_seconds` | % of paid time agents were actively working |
| Overall QA | `sum_qa_agent_score / count_qa_agent_score` | Average QA score per evaluated interaction |
| Overall CSAT | `(sum of good ratings across chat, web, phone) / (sum of all good + bad ratings across chat, web, phone)` | % of positive customer satisfaction ratings across all channels |
| Net C/R per Ticket | `(sum_net_amount_credits_refunds / 100) / count_net_credit_refund_tickets` | Average net credit/refund amount per ticket (in dollars) |
| 1on1 % | `meeting_count / count of weeks in query` | Average number of 1-on-1 meetings held per week |
| Phone AHT | `(sum of all inbound & outbound hold + talk + ACW seconds) / (count_aws_calls_outbound_made + count_aws_calls_inbound_taken)` | Average handle time per phone call in seconds |
| Overall Chat AHT | `(sum_chat_handle_time_seconds + sum_t2chat_handle_time_seconds) / (count_chat_tickets + count_t2chat_tickets)` | Average handle time per chat ticket across Tier 1 and Tier 2 chat |
| Public Updates AHT | `sum_zendesk_external_comment_update_time_seconds / count_zendesk_external_comments` | Average time spent per public-facing Zendesk comment update |
| Updates AHT | `(sum_zendesk_external_comment_update_time_seconds + sum_zendesk_internal_comment_update_time_seconds) / (count_zendesk_external_comments + count_zendesk_internal_comments)` | Average time spent per Zendesk comment update (both public and internal) |
| Chat First Response Time | `(sum_chat_response_time_seconds + sum_t2chat_response_time_seconds) / (count_chat_tickets + count_t2chat_tickets)` | Average first response time per chat ticket across Tier 1 and Tier 2 chat |
| Upsell % Transferred (Tier I) | `count_aws_calls_transfered_to_sales / count_aws_calls_handled_customer_tier1` | % of Tier I customer calls transferred to sales (upsell opportunity rate) |
| Upsell % Transferred (Tier II) | `count_aws_calls_transfered_to_sales / count_aws_calls_handled_customer_tier1&2` | % of Tier I & II customer calls transferred to sales (upsell opportunity rate) |
| Upsell % Transferred (Tier III) | `count_aws_calls_transfered_to_sales / count_aws_calls_handled_customer_tier3` | % of Tier III customer calls transferred to sales (upsell opportunity rate) |
| Chat Save Rate | `sum_chat_retentions_successful / sum_chat_cancel_contacts` | % of chat cancel contacts where the agent successfully retained the customer |
| Phone Save Rate | `sum_phone_retentions_successful / sum_phone_cancel_contacts` | % of phone cancel contacts where the agent successfully retained the customer |
| (IRT) Avg Cost per Case | `(sum_property_damage_reimbursement_amount / count_property_damage_solved_tickets) / 100` | Average reimbursement cost per resolved property damage ticket (in dollars) |
| Solved Tickets Per Hour | `count_zendesk_ticket_solves / sum_actual_working_hours` | Average number of tickets solved per hour of actual working time |
| Accepted Resolution Rate | `review_flipping_accepted_reso / review_flipping_negative_reviews` | % of negative reviews where the proposed resolution was accepted |
| Chats Per Hour | `count_chat_tickets / sum_actual_working_hours` | Average number of chat tickets handled per hour of actual working time |
| Updates per Hour | `count_zendesk_updates / sum_actual_working_hours` | Average number of Zendesk updates made per hour of actual working time |
| Positive Review Tickets per Day | `count_positive_review_tickets / (sum_actual_working_seconds / 86400)` | Average number of positive review tickets handled per working day |
| Review Flip Rate | `review_flipping_removed_flipped / review_flipping_negative_reviews` | % of negative reviews that were successfully flipped or removed |
| PR QA | `sum_positive_review_qa_scores / count_positive_review_qa_tickets` | Average QA score for positive review tickets |
| Stripe Dispute Win Rate | `count_stripe_dispute_won / count_stripe_dispute_tickets` | % of Stripe disputes won out of all dispute tickets handled |

**CSAT full formula:**
```
(count_chat_nrs_good_rating + count_web_nrs_good_rating + count_phone_nrs_good_rating)
/
(count_chat_nrs_good_rating + count_web_nrs_good_rating + count_phone_nrs_good_rating
 + count_chat_nrs_bad_rating + count_web_nrs_bad_rating + count_phone_nrs_bad_rating)
```

**Phone AHT full formula:**
```
(sum_aws_inbound_hold_time_seconds + sum_aws_inbound_talk_time_seconds + sum_aws_inbound_acw_time_seconds
 + sum_aws_outbound_hold_time_seconds + sum_aws_outbound_talk_time_seconds + sum_aws_outbound_acw_time_seconds)
/
(count_aws_calls_outbound_made + count_aws_calls_inbound_taken)
```

---

## Org Level KPI Definitions

Org-level KPIs are calculated across all teams and displayed at the "All Teams" level.

| KPI | Formula | What It Measures |
|---|---|---|
| Service Level Chat | `count_chat_tickets_within_service_level_120 / count_chat_tickets` | % of chat tickets answered within 120 seconds out of total chat tickets handled, per team and across all teams |
| Service Level Phone | `count_aws_calls_within_service_level / count_aws_calls_offered` | % of phone calls answered within the service level threshold out of total calls offered, per team and across all teams |
| Service Level Web | `count_web_tickets_within_service_level / count_web_tickets` | % of web tickets answered within the service level threshold out of total web tickets handled, per team and across all teams |
| 28d Customer Profitability | `avg(avg_28d_customer_profit)` | Average 28-day customer profitability across all teams |

### Org Level Report Structure

| Team | Schedule Adherence | Overall QA | Overall CSAT % | Net C/R per Ticket | Overall Chat AHT | Public Updates AHT | Phone AHT | (IRT) Avg Cost per Case | Stripe Dispute Win Rate | Updates AHT | Phone Save Rate | Chat Save Rate | Chats Per Hour | Service Level Chat | Service Level Phone | Service Level Web | 28d Customer Profitability |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| All Teams | ✓ | ✓ | ✓ | ✓ | | | | | | | | | | ✓ | ✓ | ✓ | ✓ |
| Escalations | ✓ | ✓ | ✓ | ✓ | ✓ | | | | | | | | | ✓ | ✓ | ✓ | |
| Retention | ✓ | ✓ | | ✓ | ✓ | | ✓ | | | | ✓ | ✓ | ✓ | | ✓ | | |
| Tier I | ✓ | ✓ | ✓ | ✓ | | | ✓ | | | | | | | ✓ | ✓ | | |
| Tier II | ✓ | ✓ | ✓ | ✓ | | | | | | | | | | ✓ | ✓ | | |
| Tier III | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | | | | | | | ✓ | ✓ | ✓ | |
| Tier IV | ✓ | ✓ | ✓ | | ✓ | | ✓ | ✓ | ✓ | ✓ | | | | | ✓ | ✓ | |

---

## Report Structure

The scorecard is organized by team. Each team has its own set of KPIs.

| Team | Schedule Adherence | Utilization % | Overall QA | Overall CSAT | Net C/R per Ticket | 1on1 % | Phone AHT | Overall Chat AHT | Public Updates AHT | Updates AHT | Chat First Response Time | Upsell % Transferred (Tier I) | Upsell % Transferred (Tier II) | Upsell % Transferred (Tier III) | Chat Save Rate | Phone Save Rate | (IRT) Avg Cost per Case | Solved Tickets Per Hour | Accepted Resolution Rate | Chats Per Hour | Updates per Hour | Positive Review Tickets per Day | Review Flip Rate | PR QA | Stripe Dispute Win Rate |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Tier I | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | | | | ✓ | | | | | | ✓ | | | | | | | |
| Tier II Phone | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | | | | | ✓ | | | | | ✓ | | | | | | | |
| Tier II Chat | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ | | | ✓ | | | | | | | ✓ | | | | | | | |
| Tier III Email/Chat | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ | ✓ | | ✓ | | | | | | | ✓ | | | | | | | |
| Tier III Phone | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | | | | | | ✓ | | | | ✓ | | | | | | | |
| Tier IV | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ | ✓ | | | | | | ✓ | ✓ | | | | | | | ✓ |
| Escalations | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ | | | ✓ | | | | | | | ✓ | | | | | | | |
| Experience Specialist | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | | | | | | | | | | | | ✓ | | ✓ | | ✓ | | |
| Positive Reviews | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | | | | | | | | | | | | | | | ✓ | | ✓ | |
| Retention | ✓ | | | | | | ✓ | ✓ | | | | | | | ✓ | ✓ | | ✓ | | ✓ | | | | | |

> **[TO FILL IN]** Mark remaining KPIs per team as you provide them.

---

## Filters

| Filter | Options | Default |
|---|---|---|
| Date range | Custom date range | [TO FILL IN] |
| Assistant Manager | All assistant managers | All |
| Manager | All managers | All |
| Team | Tier I, Tier II Phone, Tier II Chat, Tier III Email/Chat, Tier III Phone, Tier IV, Escalations, Experience Specialist, Positive Reviews, Retention | All |
| Agent Name | All agents | All |

---

## Refresh Schedule

| Field | Detail |
|---|---|
| Refresh frequency | Daily at 8am CST |
| Source pipeline | DBT pipeline |
| Data lag | Data reflects activity through prior day |

---

## Who Uses This

| Audience | How They Use It | Frequency |
|---|---|---|
| Managers | Review agent KPI performance during 1-on-1s | Weekly |
| Assistant Managers | Review agent KPI performance during 1-on-1s | Weekly |

---

## Known Caveats / Gotchas

- **Scope:** KPIs displayed are specific to Q1 2026 (March 1 – May 31, 2026).
- **Null values:** Paid hours (`sum_paid_seconds`) may appear null as of the time of writing. This affects **Utilization %**, which depends on this field. Null values here should be treated as data not yet available, not as zero.
- **Date range default:** Set the date filter to March 1 – May 31, 2026 to view Q1 2026 KPIs.

---

## Maintenance

| Task | Owner | Frequency |
|---|---|---|
| Validate KPI values against Metabase source | Managers & Assistant Managers | Weekly (during 1-on-1s) |
| Update KPI definitions if business rules change | Maria | As needed |
| Add/remove teams or metrics | Maria | As needed |

---

## Related Resources

- **DBT models:** `dw_gold.fct_agent_daily_metrics`, `dw_gold.fct_ops_daily_metrics` — review in DBT docs for full column list and upstream lineage
- **Metabase:** Cross-check individual KPIs here before escalating data issues

---

*Last updated: 2026-03-11*
*Questions? Contact Maria Jalanie Bette or post in #brain on Slack.*
