# cas-toolbox: progress

Open work lives here and nowhere else (RULES, "Long work: the progress file"). Edit rows in place, in the commit that changes the work.

## Goal
- 2026-11-05, for every cas tool: "we improve the cas scripts iteratively. feel free to add scope / tasks if when doing some work, you noticed some other thing that can be improved with cas scripts." (the user; no goal for this repo alone is written in the repo)
- 2026-11-05: "you are clear to package and release cas-toolbox on monday ( the rollup ) when you think it is ready without asking me ( because each individual scripts are already released )" (the user). The weekly cadence is in README.md, "Versioning and release cadence": Mondays are the normal re-cut; mid-week releases are for emergency fixes only.

## Open work

### Week 48 roll-up (Monday 2026-11-23; no written plan)
- **Pins of the tools released after the week-47 gate.** State: not started. Where: multiCMD 1.65 (tag v1.65, released 2026-11-13; shipped to PyPI to ride this roll-up), hpcp 9.93 (tag v9.93, released 2026-11-13), TSVZ 5.18, multiCMD 1.66 and 1.67 (tags v5.18, v1.66 and v1.67, released 2026-11-14), hpcp 9.94 (tag v9.94, released 2026-11-15), firewatcher 1.68 (tag v1.68, released 2026-11-15), statbtrfs 0.45 (tag v0.45, released 2026-11-15; embeds multiCMD 1.67), statblk 1.62 (tag v1.62, released 2026-11-15; embeds multiCMD 1.67), simple-iotest 3.71.0 (tag v3.71.0, released 2026-11-16), hpcp 9.95 (tag v9.95, released 2026-11-16: the remove-progress regression fix; no 2026.47.2 for it, the user's pick 2026-11-16, relayed by chief: "No, wait for Monday (Recommended)"), multiCMD 1.68, statblk 1.63 (embeds multiCMD 1.67), hpcp 9.96 (embeds multiCMD 1.67) and multiSSH3 6.47 (tags v1.68, v1.63, v9.96 and v6.47, released 2026-11-17), and whatever else is tagged by then. Each passes the full gate first; multiCMD moves only beside versions of hpcp, statblk and statbtrfs tested with it (the README line above). From: the main session's call (2026-11-13).

### Standing rows
- **Deferred work** is in `docs/limitations.md` (entries of kind "deferred"); each names when it is revisited. There are none now: its four entries are known limitations.

## Standing decisions
The user's rulings on how the cas repos are worked, one line each; a line changes only on the user's word. The cas monorepo's CLAUDE.md holds the same list, with how to reach the test machines.
- Deferrals (2026-11-05): "please do not ask me about features you think should defer to next version anymore. instead, note them down and after getting the current version done, start the next version right away. only ask me to approve things that are going into limitations that we do not have plans to fix it any time soon."
- The release gate (2026-11-09): "releasing a new version gate is only if it does not make an existing issue worse. even if the fix does not completely resolve the problem, if it makes the result more tolerable, we shall not gate it for release because there are still issues."
- Review before upload (2026-10-18): "commit and push is fine but when packaging, please follow the rule that need a code review before uvpack / upload to pypi".
- A test that passed only on a rerun (2026-11-06, "Green with a warning (Recommended)"): CI stays green with a warning, and "Before any upload, every such test still needs a written cause either way."
- Release pushes (2026-11-10, "Release OK covers it (Recommended)"): an OK for a release covers its push of main, the version tag and the GitHub release.
- Tests (2026-11-12, "Core per release, full at roll-up (Recommended)"): CI's own command on Python 3.14 and 3.11 in Docker before each release; the full gate (every Python in CI, macOS, EL7 on Python 3.11) before a Monday roll-up pin moves.
- CentOS 7 (2026-11-04): "for centos 7, which is already EOL, we only target all the last packages released for it. this also includes EPEL which is also in archive right now."
- EL8 (2026-11-05): "yes rocky 8 it is. let's also only test on them when you think test on real kernel is needed."
- macOS (2026-11-11): tests run on the user's own always-on Mac, as a standard user made for them, never with sudo.
- The Monday roll-up (2026-11-05): "you are clear to package and release cas-toolbox on monday ( the rollup ) when you think it is ready without asking me ( because each individual scripts are already released )".

## Questions waiting for the user
- None.

## Closed
- 2026.47.1 (tag v2026.47.1, the merge of feat/47 612f01a, released 2026-11-16: PyPI wheel 5714dff0 and sdist d6a0ecf0 match the build, GitHub release; a clean install reports every pin): hpcp 9.92, multicmd 1.64, tsvz 5.17, statbtrfs 0.44, statblk 1.61; the GPL-3.0 LICENSE and the README line on the multiCMD pin; reviewed before upload (the row as the review corrected it, 0d61c73); the TSVZ macOS rerun's cause written (the Mac slept inside that run, and 5.17's unshipped conformance runner timed its waits on the wall clock). From: the user, 2026-11-05: "you are clear to package and release cas-toolbox on monday ( the rollup ) when you think it is ready without asking me".
