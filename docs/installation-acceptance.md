# Independent installation acceptance

## Scope and decisions

This checks installation autonomy in a fresh Codex desktop task. It does not measure the downstream value of derived skills. The primary Agent judges the evidence; deterministic checks cover files and scheduler records only. The independent task receives only the user's repository installation request. No evaluation hints or reminders are sent to it.

The existing host automation tool is reused. First-install behavior is documented outside the installed skill, so recurring runs do not load setup instructions. Default configuration avoids extra questions; users can disable the resulting schedule. Login and actual platform restrictions remain user/external-state dependencies.

## Baseline — FAIL

Repository revision: `a26b02d`. The independent task found an existing identical installation, ran local checks successfully and tried the real source. It encountered a platform IP-risk page and ended without creating a recurring task. Its own final message explicitly stated that scheduling was not enabled. The environment retained private source configuration from earlier work, so this baseline is not a clean-machine installation.

## Change and acceptance criteria

Make the default first-install responsibility explicit: install, create and read back the daily task, persist its ID, attempt source reading and notify the user. Use host local time at 18:00 unless an existing preference says otherwise. Source login failure must not block independent scheduling work. Respect an explicit user opt-out or an already disabled task. Routine runtime skill context excludes this onboarding document.

Acceptance requires actual installation evidence, a real scheduler record and saved ID, a bounded source attempt, and no unnecessary permission question. A login/platform block may be reported honestly; it is not counted as successful content collection. Real collection and learned-method effectiveness remain separate claims.

Retest pending.
