# cas-toolbox: progress

Open work lives here and nowhere else (RULES, "Long work: the progress file"). Edit rows in place, in the commit that changes the work.

## Goal
- 2026-11-05, for every cas tool: "we improve the cas scripts iteratively. feel free to add scope / tasks if when doing some work, you noticed some other thing that can be improved with cas scripts." (the user; no goal for this repo alone is written in the repo)
- 2026-11-05: "you are clear to package and release cas-toolbox on monday ( the rollup ) when you think it is ready without asking me ( because each individual scripts are already released )" (the user). The weekly cadence is in README.md, "Versioning and release cadence": Mondays are the normal re-cut; mid-week releases are for emergency fixes only.

## Open work

### Week 49 roll-up (Monday 2026-11-30; no written plan)
- **Pins of the tools released after the week-48 gate.** State: not started; none released since. Where: each tool's next version as its own progress file has it (hpcp 9.97, multiCMD 1.70, multiSSH3 6.48, simple-iotest 3.72.0, statblk 1.65, firewatcher 1.69; TSVZ 5.19 is paused at the user's ask); each passes the full gate first, and multiCMD moves only beside versions of hpcp, statblk and statbtrfs tested with it. From: the user, 2026-11-05: "you are clear to package and release cas-toolbox on monday ( the rollup ) when you think it is ready without asking me".

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
- 2026.48.1 (tag v2026.48.1, the merge of feat/48 1781205, released 2026-11-23: PyPI wheel d464e27d and sdist ba083459 match the build, GitHub release): hpcp 9.96, multicmd 1.69, multissh3 6.47, simple-iotest 3.71.0, statbtrfs 0.45, statblk 1.64, tsvz 5.18, firewatcher 1.68; tee-logger 6.45 unchanged. Each passed the full gate (Docker on every CI Python, EL7 on 3.11, hpcp's root suite, the Mac, main's CI; the Mac runs of multiSSH3 6.47 and simple-iotest 3.71.0, and TSVZ 5.18's main CI, were judged no worse than the pinned versions). TSVZ 5.18 by chief's decision C205 of 2026-11-23 (not the user's words; the question: main's 3.6 CI job red from a race 5.17 has as often, 11 and 12 failures in 3,000 runs), on chief's condition that 5.18 is not worse than 5.17 on the approved Ctrl-C figures: on Python 3.9, 960 caught Ctrl-Cs a run around get_file_obj() and release_file_obj(), side by side, 5.17 hung 296 and 287 times and 5.18 10 and 16 (approved 8 to 25; landings between the two functions' steps 3 to 4, approved 1 to 6). Reviewed before upload (review, three wording rounds, re-reads: release); the TSVZ part from the reviewed drop-in. From: the user, 2026-11-05: "you are clear to package and release cas-toolbox on monday ( the rollup ) when you think it is ready without asking me".
- 2026.47.1 (tag v2026.47.1, the merge of feat/47 612f01a, released 2026-11-16: PyPI wheel 5714dff0 and sdist d6a0ecf0 match the build, GitHub release; a clean install reports every pin): hpcp 9.92, multicmd 1.64, tsvz 5.17, statbtrfs 0.44, statblk 1.61; the GPL-3.0 LICENSE and the README line on the multiCMD pin; reviewed before upload (the row as the review corrected it, 0d61c73); the TSVZ macOS rerun's cause written (the Mac slept inside that run, and 5.17's unshipped conformance runner timed its waits on the wall clock). From: the user, 2026-11-05: "you are clear to package and release cas-toolbox on monday ( the rollup ) when you think it is ready without asking me".
