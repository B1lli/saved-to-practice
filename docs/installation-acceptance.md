# Independent installation acceptance

## Scope and decisions

This checks installation autonomy in a fresh Codex desktop task. It does not measure the downstream value of derived skills. The primary Agent judges the evidence; deterministic checks cover files and scheduler records only. The independent task receives only the user's repository installation request. No evaluation hints or reminders are sent to it.

The existing host automation tool is reused. First-install behavior is documented outside the installed skill, so recurring runs do not load setup instructions. Default configuration avoids extra questions; users can disable the resulting schedule. Login and actual platform restrictions remain user/external-state dependencies.

## Baseline — FAIL

Repository revision: `a26b02d`. The independent task found an existing identical installation, ran local checks successfully and tried the real source. It encountered a platform IP-risk page and ended without creating a recurring task. Its own final message explicitly stated that scheduling was not enabled. The environment retained private source configuration from earlier work, so this baseline is not a clean-machine installation.

## Change and acceptance criteria

Make the default first-install responsibility explicit: install, create and read back the daily task, persist its ID, attempt source reading and notify the user. Use host local time at 18:00 unless an existing preference says otherwise. Source login failure must not block independent scheduling work. Respect an explicit user opt-out or an already disabled task. Routine runtime skill context excludes this onboarding document.

Acceptance requires actual installation evidence, a real scheduler record and saved ID, a bounded source attempt, and no unnecessary permission question. A login/platform block may be reported honestly; it is not counted as successful content collection. Real collection and learned-method effectiveness remain separate claims.

## Retest — FAIL for complete autonomous setup

Repository revision: `42d53bf`. The skill entry was removed before a fresh projectless desktop task received the same installation request. Private profile and browser state were retained. Installation succeeded (including recovery from a downloader certificate failure using the supported Git transport), the local suite passed 12 checks with one optional skip, and the task read a real three-image note. It did not create a daily task and selected an external browser rather than demonstrating the requested in-app-first route.

After the acceptance turn ended, a separate read-only diagnostic checked the tool registry: 266 tools were exposed, with no `automation_update` or automation/heartbeat/scheduler-named tool. The diagnostic is not part of the acceptance run. This is evidence of a missing tool in that task, not evidence that the whole desktop app lacks scheduling.

The parent also attempted the documented `cua.createBrowserTab("iab", ..., {visible: true})` entry directly; the control call timed out even while the UI showed an in-app tab. An open UI tab is not proof of a working control connection.

## Follow-up repair

The source guide now specifies a direct in-app-browser attempt before Playwright, and rejects treating global browser inventory failure as proof that the in-app browser is unavailable. First-install guidance explicitly checks actual tool discovery before declaring scheduling unavailable. No setup instructions were added to recurring runtime context.

Validation: 12 local checks passed; the optional real-browser check is skipped without its runtime. The skill validator and diff checks passed. Full autonomous setup remains blocked by host browser-control and task tool exposure; no successful scheduler creation or passing third independent run is claimed. Raw account data and task logs remain private.
