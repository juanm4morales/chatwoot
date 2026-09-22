# Conversation monitors: final stacked PR plan

Source: [PR #15962](https://github.com/chatwoot/chatwoot/pull/15962), frozen at `562de2bccef4ff9b13179a35e08cb09971971c4c`. Ticket: [CW-8270](https://linear.app/chatwoot/issue/CW-8270/add-conversation-monitors-to-reports-with-jev).

The stack starts from develop at `8aa1fafe2034d69af8e1b2c9ace74b9dfb1d878d`. Each draft targets the preceding numbered branch; only PR 1 targets develop. This is a review reorganization, with the same final product behavior and five-table schema. Existing migration identities are preserved. The original PR remains open as a reference and receives the replacement map.

## Final boundaries

| Order / branch suffix | Review concern | Acceptance and focused verification | Review scope |
| --- | --- | --- | --- |
| 01-foundation | Account-owned definitions, evaluations, scan intervals, durable work and usage schema; default-off flag; associations and foundational model invariants. | Fresh isolated database migrates to all five final tables. Feature-bit mapping, account model specs, CE/EE boot, foreign keys and uniqueness remain valid. No source callbacks or user entry point. | L: migrations, models, flag are separate bounded extraction tasks. |
| 02-credits | Atomic 100,000 monthly calls, daily UTC accounting, hit timestamp, Redis capacity reservation, and authorized monitor.updated broadcasts. | Usage and broadcast specs verify last-credit behavior, reset/history, account isolation and viewer authorization. No provider is invoked yet. | M |
| 03-openrouter | Shared installation configuration, public context construction, Jev System One requests, validation/error handling and benchmark fixtures. | Context/client/config specs cover latest-five vs full history, Unicode/28 KB trimming, 0.60 threshold configuration, 60 KB request limit, provider errors and credit accounting. Coordinate canonical CAPTAIN_OPENROUTER_* settings with PR #15906. | M: provider and context are separate review commits if needed. |
| 04-evaluation | Durable scheduler, unified scan execution, evaluator/result fencing, retry and recovery jobs. | Explicitly schedule work in engine tests before source callbacks exist. Verify batching/splitting, sticky matches, stale input/collection versions, disabled flags, canceled scans, failed enqueue recovery and quota holds. | L: engine and durable scan/recovery are separate extraction tasks. |
| 05-source-events | Public customer/agent callbacks, redaction/deletion invalidation, commit-time activation, import completion, and operational queue/schedule wiring. | Restore the complete evaluator integration suite; run existing message/conversation/import and scheduling regressions. Private notes and disabled accounts must not invoke the provider. | M |
| 06-lifecycle | Pause/resume and condition updates, plus the coverage/bucket/presenter contracts those transitions affect. | Resume/update/bucket specs verify catch-up versus future-only, old conversations receiving replies, skipped/canceled history warnings, immutable thresholds, rechecks and stale actions. | L: transitions and coverage each have their own acceptance checkpoint. |
| 07-api | Full account-scoped management, async preview, chart and drilldown API; account reporting timezone. | Request specs verify feature/permission boundaries, input validation, preview cooldown, UTC usage snapshots, timezone/bucket consistency, stale drilldowns and readable history at quota. All referenced services already exist. | M |
| 08-report-view | API client, direct detail route, graph/filter/drilldown, quota/coverage presentation, and realtime recovery. | Chart/date/navigation, usage, refresh and ActionCable tests pass. A monitor created by API has a working report URL. Keep management controls and list navigation out until their UI arrives. | L: supporting components/realtime and graph rendering are separate extraction tasks. |
| 09-create-ui | Sidebar discovery, list/empty state, creation and preview form, pagination and detail back navigation. | Form tests and route/build checks verify a complete create-to-chart flow, cooldown/error states and account feature gating. Reuse existing quota and realtime helpers. | M |
| 10-manage-ui | Edit, duplicate, pause/resume, delete and retry controls, plus final product/rollout documents. | Complete UI/backend suites, lint, production build and final tree-equivalence check. Both resume choices, condition edits and stale navigation behavior retain the reference tests. Documentation is a separate review unit from management code. | M code; documentation reviewed separately. |

Branches use `codex/cw-8270-<suffix>` from the table. Tests travel with the behavior they exercise. Earlier branches include only methods whose dependencies exist; later branches restore the exact reference implementations. No placeholder classes, disabled tests, or temporary no-op guards are used to conceal dependencies.

## Checkpoints

1. After 3: schema, feature flag, context and credit contracts are independently testable; collection remains off.
2. After 5: source changes can safely drive the durable evaluation engine and recover missed enqueueing.
3. After 7: all backend product flows are available through authorized APIs, before dashboard discovery.
4. After 10: full feature equivalence, preserved migrations, focused regression suites, frontend build and clean commits are verified before draft publication.

## Compatibility and rollout

Keep all eight migrations and the legacy Jev model-ID mapping. Shared/preview deployment has not been ruled out, so do not rewrite applied migration history. For upgrades from the earlier prototype, use the data-preserving scan migration and documented worker/legacy-queue transition. The feature remains disabled by default and should only be enabled after the full stack has merged.

The current develop changes are retained. Final equivalence is checked against the clean merge of the frozen feature head onto the recorded develop commit; the stack plan and delivery links are the only intended additional documentation.

## Review and merge procedure

Every draft includes the whole stack map, parent/successor, original reference, CW-8270 and product-oriented checks. Review and merge in ascending order. A child targets its predecessor only to keep the diff small: do not merge the child into that feature branch as the delivery action.

After a parent lands in develop, replay only the child's own changes on updated develop and retarget it; with squash merges, avoid reintroducing the parent's changes. Refresh descendants in order and rerun affected validation. Do not delete predecessor branches while descendants still target them. Recheck shared provider config and feature-bit allocation against develop before merging.

All PRs are drafts. Creation does not authorize merge, rollout, or closing the original reference PR. No product decisions remain open for this extraction.


## Published draft stack

| Order | Draft PR | Incremental diff |
| --- | --- | --- |
| 1 | [#15966: define monitor data contracts](https://github.com/chatwoot/chatwoot/pull/15966) | 22 files changed, 525 insertions(+), 2 deletions(-) |
| 2 | [#15967: enforce monitor call credits](https://github.com/chatwoot/chatwoot/pull/15967) | 6 files changed, 294 insertions(+) |
| 3 | [#15968: evaluate monitor conditions through OpenRouter](https://github.com/chatwoot/chatwoot/pull/15968) | 13 files changed, 536 insertions(+), 1 deletion(-) |
| 4 | [#15969: process durable monitor evaluations](https://github.com/chatwoot/chatwoot/pull/15969) | 10 files changed, 653 insertions(+) |
| 5 | [#15970: track monitor source activity](https://github.com/chatwoot/chatwoot/pull/15970) | 10 files changed, 275 insertions(+), 3 deletions(-) |
| 6 | [#15971: support monitor lifecycle transitions](https://github.com/chatwoot/chatwoot/pull/15971) | 9 files changed, 686 insertions(+) |
| 7 | [#15972: expose conversation monitor APIs](https://github.com/chatwoot/chatwoot/pull/15972) | 6 files changed, 549 insertions(+) |
| 8 | [#15973: display live monitor reports](https://github.com/chatwoot/chatwoot/pull/15973) | 13 files changed, 1203 insertions(+) |
| 9 | [#15974: create and preview conversation monitors](https://github.com/chatwoot/chatwoot/pull/15974) | 7 files changed, 597 insertions(+) |
| 10 | [#15975: manage conversation monitors](https://github.com/chatwoot/chatwoot/pull/15975) | 5 files changed, 836 insertions(+), 8 deletions(-) |

The report-view PR contains two review commits: supporting components/realtime (442 added lines) and the graph/detail page (761 added lines). The management PR separates its product code from the 501-line original design/rollout documents. These are the two larger PRs; the other incremental diffs are 275–686 lines before this final delivery note.

## Final validation

- 530 backend examples and 38 frontend tests passed on the complete stack.
- Ruby lint passed across 60 files. Frontend lint passed across 17 files with zero errors and eight known translation/root-condition warnings.
- The production frontend build passed, with existing bundle-size/dependency warnings.
- Fresh-database migrations, final schema uniqueness/default-off checks, and both Community/Enterprise Zeitwerk checks passed.
- Each intermediate branch passed its focused checks before committing, with repository hooks enabled.
- The complete feature tree is identical to the clean merge of reference commit `562de2bccef4ff9b13179a35e08cb09971971c4c` onto recorded develop `8aa1fafe2034d69af8e1b2c9ace74b9dfb1d878d`, apart from this stack-plan document. No feature behavior or applied migration IDs were changed by extraction.

GitHub CI runs separately; local validation does not imply all remote checks have completed. Keep the original PR open as a reference while reviewing these drafts.
