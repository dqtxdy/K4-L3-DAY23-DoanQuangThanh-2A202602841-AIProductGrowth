# UNIPILOT · OPERATING DASHBOARD

**B2C tự phục vụ / PLG đề xuất** · **09/10/2026** · Đoàn Quang Thanh · 2A202602841  
Sinh viên dự kiến vừa dùng vừa trả thuê bao; chưa xác minh khách trả phí, tổ chức mua hay partner phân phối.

## NORTH STAR

**Weekly On-Time Important Task Completions** — task học tập important do student capture và xác nhận complete trước/đúng hạn trong academic week. Completion phải persist non-completed→completed, đúng task/user; loại duplicate, save fail, task đã complete, click/login, AI plan delivery. **Hiện tại: CHƯA ĐO · mục tiêu chưa phê duyệt.**

## Metric lights · mọi số “hiện tại” đều CHƯA ĐO

| Tier / metric | Ngưỡng 🟢 xanh · 🟡 vàng · 🔴 đỏ | Nguồn, kỳ & báo trước |
|---|---|---|
| **LEADING · Eligible-week value retention / curve** W4; taskless/break weeks không churn | 🟢 W4 ≥30% và W2→W4 drop ≤5pp qua 2 cohorts · 🟡 W4 15–<30% hoặc drop >5pp · 🔴 W4 <15% qua 2 cohorts | [TB] W4 eligible academic weeks; n≥10/cohort, mature; <30% is Day22 proposal, not benchmark. Báo trước paid retention/LTV; CHƯA ĐO. |
| **LEADING · 7-day first-value Activation** first task→persisted completion | 🟢 ≥40% · 🟡 25–<40% · 🔴 <25% | [TB] ≥20 users with 7-day follow-up; báo trước eligible W1 retention. 7 days = Day20 hypothesis. |
| **LEADING · Upcoming-task readiness** captured due tasks with valid due date + actionable context | 🟢 ≥80% · 🟡 60–<80% · 🔴 <60% | [TB] ≥30 due tasks/week ×2; báo trước activation/NSM (hypothesis). |
| **OPERATING · Priority-plan adoption-to-value** presented tasks later completed on time | 🟢 ≥50% · 🟡 25–<50% · 🔴 <25% | [TB] ≥20 eligible tasks/week ×2 and followed through due date; predicts NSM, not AI quality. |
| **OPERATING · All-in direct Cost/credit** | 🟢 ≤$0.3992 · 🟡 >$0.3992–$0.4990 · 🔴 >$0.4990 | [MH] USD/credit, ≥30 credits + complete invoice allocation. $0.499 all-in ceiling from 60% GM; $0.47536 is infrastructure-only ceiling. |
| **LAGGING · Net CAC / net new paid seat** | 🟢 ≤$9.3491 · 🟡 >$9.3491–$11.68632 · 🔴 >$11.68632 | [MH] $11.68632 cap = 3-month payback; ≥10 net paid seats, spend/refunds complete. Actual CAC CHƯA ĐO. |
| **LAGGING · Paid-seat GM** | 🟢 ≥70% · 🟡 60–<70% · 🔴 <60% | [MH] 60% model floor; ≥10 complete paid seat-months. Model GM 78.0649% is estimate, not actual. |

Missing/zero denominator, immature cohort, incomplete cost attribution or sample guard failure = **CHƯA ĐO**, never green/red. Weekly retention cohorts enter on first valid `academic_task_completed`; return requires another such event during an eligible active teaching week. Events are defined in Day20; actual telemetry is not available.

## 5 operating rules · AI-assisted drafts, student rewrite required

1. **⏹** If eligible W4 <15% in 2 mature cohorts (n≥10 each), freeze paid-acquisition launch and run 5 moderated first-task→completion sessions next week; do not buy ads to offset churn or count taskless weeks as churn.
2. **⏹** If 7-day activation <25% in 2 cohorts (n≥20 each; event QA ≥95%), pause paywall work and fix one onboarding blocker, then run 10 task-flow sessions; do not discount to hide failure to reach first value.
3. **⏹** If all-in cost >$0.499/credit for a closed month (≥30 requests and reconciled costs), freeze allowance expansion and cap test build at 4 credits; do not report modeled $0.27364 as measured or expand quota.
4. If readiness <60% for 2 academic weeks (≥30 due tasks/week; event QA ≥95%), add due-date/context validation next sprint; do not infer missing deadlines with AI and call tasks ready.
5. **⏹** If net CAC >$11.68632 in a closed acquisition cohort (≥10 net paid seats with full spend/refunds), freeze paid acquisition launch/expansion and run tagged organic/referral only next week; with no transactions, keep NO-GO. Do not scale from the $7.50 stretch scenario or competitor pricing.

## 30 / 60 / 90-day gates · one gating metric each

| Gate / date | Single pass metric | Physical evidence · decision if fail |
|---|---|---|
| **D30 · 08/11/2026 · Learning** | ≥8 of 12 interviewed students independently report recurring weekly overlapping-deadline pain and show current workaround | 12 consented, anonymized notes + coding rubric. Pass: continue learning; fail: FIX ICP/problem. Not revenue. |
| **D60 · 08/12/2026 · Operating** | 7-day Activation ≥40%; n≥10 with full follow-up and ≥95% event QA | Timestamped event export + cohort query/QA log. Pass: bounded retention cohort; fail/low n: FIX workflow/instrumentation, no paid scaling. |
| **D90 · 07/01/2027 · Viability** | Eligible academic W4 retention ≥30%; n≥10 mature students with actionable work | Cohort table + eligibility audit + QA. Pass: consider next experiment subject to economics controls; 10–<30% FIX/PIVOT once. CAC is a non-gating NO-GO control until ≤$11.68632 observed. |

**KILL (draft):** On 07/01/2027, stop current B2C positioning if eligible W4 retention <10% among ≥10 mature students after one documented FIX and event QA ≥95%. Insufficient follow-up/sample = CHƯA ĐO, not KILL.

**CHƯA ĐO ĐƯỢC:** Không có users/interviews đã hoàn thành, paid seats, CAC, billing/refunds, retention, production LLM/token or hosting invoices, free-tier cost, p95, WTP hay runway. Day22 100/100 synthetic validator pass ≠ AI/user success; R=85%, $0.27364/credit, $4.99, 4 credits and 78.0649% GM là model estimates/proposals. Cần privacy/security-approved event, cost and billing instrumentation; first baseline after proposed 15/11/2026 review. Proposed price/credit and failure policy are not approved. Ngưỡng/rules/gates là AI-assisted drafts awaiting student rewrite and sign-off.
