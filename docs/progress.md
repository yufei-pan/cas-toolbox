# cas-toolbox: progress

Open work lives here and nowhere else (RULES, "Long work: the progress file"). Edit rows in place, in the commit that changes the work.

## Goal
- 2026-11-05, for every cas tool: "we improve the cas scripts iteratively. feel free to add scope / tasks if when doing some work, you noticed some other thing that can be improved with cas scripts." (the user; no goal for this repo alone is written in the repo)
- 2026-11-05: "you are clear to package and release cas-toolbox on monday ( the rollup ) when you think it is ready without asking me ( because each individual scripts are already released )" (the user). The weekly cadence is in README.md, "Versioning and release cadence": Mondays are the normal re-cut; mid-week releases are for emergency fixes only.

## Open work

### Week 47 roll-up, version 2026.47.1 (Monday 2026-11-16; no written plan)
- **LICENSE file and the README line on the multiCMD pin.** State: merged (2026-11-15). Where: branch feat/47 (worktree cas-toolbox-47), a42c4d0 on master c7317a9: adds the GPL-3.0 LICENSE and replaces the README dependency-graph bullet "multiCMD is the foundation" with "hpcp, statbtrfs and statblk carry their own copy of multiCMD and use an installed one when it is at least as new, so the multiCMD pin is what they run in the toolbox: move it only beside versions of them tested with it". Merged at 612f01a, not pushed (the upload pushes it). From: the main session's plan.
- **The week's pins.** State: merged (2026-11-15): pyproject.toml, the README table and uv.lock. Where: pyproject.toml and the README table "Included tools": hpcp 9.84 to 9.92, multicmd 1.58 to 1.64, tsvz 5.9 to 5.17, statbtrfs 0.43 to 0.44, statblk 1.60 to 1.61. Unchanged: multissh3 6.46, simple-iotest 3.70.1, tee-logger 6.45, firewatcher 1.67. The tags v9.92, v1.64, v5.17, v0.44 and v1.61 exist in the tools' repos. A pin moves only after its tool passes the full gate (every Python in its GitHub CI matrix serially, macOS, an EL7 host on Python 3.11, hpcp's root suite; main's last CI run green or each red job's cause found outside the pinned code). Per the main session's notes the gate passed for TSVZ 5.17, multiCMD 1.64, statblk 1.61 and statbtrfs 0.44 (2026-11-13), and for hpcp 9.92 except an EL7 spawn traceback that 9.88 and 9.91 have too (deferred in hpcp). Before the upload: a test that passed only on a rerun (a TSVZ conformance run on macOS, once) needs its cause written down. multiCMD 1.65 and hpcp 9.93 (both released 2026-11-13) are not pinned this week. From: the user's picks 2026-11-13, "Quick hpcp 9.92 first (Recommended)" and "Pin 9.92 (Recommended)".
- **Version, date, release row and lock file.** State: merged (2026-11-15; the row as the pre-upload review corrected it). Where: pyproject.toml `version = "2026.46.1"` to `"2026.47.1"`; README "Current version" and "Released" (2026-11-16) and a new first row in "Recent releases" for the tools that changed (as 5ce69bd did for week 46); uv.lock refreshed. From: README cadence.
- **Release 2026.47.1.** State: merged into master (2026-11-15), reviewed, upload on Monday 2026-11-16 as the cadence says. The TSVZ macOS rerun's cause is written (not TSVZ code: the Mac slept 15:02 to 15:18 inside that run, so the run was void by the Mac rule, and 5.17's conformance runner, which is not shipped, timed its waits on the wall clock; fixed in 5.18, 18230f5). Next, Monday: record the approval again (the review covered 612f01a; since then only docs/progress.md changed, which the sdist leaves out, so the gate's fingerprint no longer matches), then `uvpack`, compare the wheel with PyPI's, annotated tag v2026.47.1, push master and the tag, `gh release create` listing the tools that changed, watch CI.

### Week 48 roll-up (Monday 2026-11-23; no written plan)
- **Pins of the tools released after the week-47 gate.** State: not started. Where: multiCMD 1.65 (tag v1.65, released 2026-11-13; shipped to PyPI to ride this roll-up), hpcp 9.93 (tag v9.93, released 2026-11-13), TSVZ 5.18, multiCMD 1.66 and 1.67 (tags v5.18, v1.66 and v1.67, released 2026-11-14), hpcp 9.94 (tag v9.94, released 2026-11-15), firewatcher 1.68 (tag v1.68, released 2026-11-15), statbtrfs 0.45 (tag v0.45, released 2026-11-15; embeds multiCMD 1.67), statblk 1.62 (tag v1.62, released 2026-11-15; embeds multiCMD 1.67), and whatever else is tagged by then. Each passes the full gate first; multiCMD moves only beside versions of hpcp, statblk and statbtrfs tested with it (the README line above). From: the main session's call (2026-11-13).

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
- (none yet)
