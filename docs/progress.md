# cas-toolbox: progress

Open work lives here and nowhere else (RULES, "Long work: the progress file"). Edit rows in place, in the commit that changes the work.

## Goal
- 2026-11-05, for every cas tool: "we improve the cas scripts iteratively. feel free to add scope / tasks if when doing some work, you noticed some other thing that can be improved with cas scripts." (the user; no goal for this repo alone is written in the repo)
- 2026-11-05: "you are clear to package and release cas-toolbox on monday ( the rollup ) when you think it is ready without asking me ( because each individual scripts are already released )" (the user). The weekly cadence is in README.md, "Versioning and release cadence": Mondays are the normal re-cut; mid-week releases are for emergency fixes only.

## Open work

### Week 47 roll-up, version 2026.47.1 (Monday 2026-11-16; no written plan)
- **LICENSE file and the README line on the multiCMD pin.** State: building. Where: branch feat/47 (worktree cas-toolbox-47), a42c4d0 on master c7317a9: adds the GPL-3.0 LICENSE and replaces the README dependency-graph bullet "multiCMD is the foundation" with "hpcp, statbtrfs and statblk carry their own copy of multiCMD and use an installed one when it is at least as new, so the multiCMD pin is what they run in the toolbox: move it only beside versions of them tested with it". Not pushed, not merged. The rows below go on this branch. From: the main session's plan.
- **The week's pins.** State: not started. Where: pyproject.toml and the README table "Included tools" (feat/47 still has the week-46 pins): hpcp 9.84 to 9.92, multicmd 1.58 to 1.64, tsvz 5.9 to 5.17, statbtrfs 0.43 to 0.44, statblk 1.60 to 1.61. Unchanged: multissh3 6.46, simple-iotest 3.70.1, tee-logger 6.45, firewatcher 1.67. The tags v9.92, v1.64, v5.17, v0.44 and v1.61 exist in the tools' repos. A pin moves only after its tool passes the full gate (every Python in its GitHub CI matrix serially, macOS, an EL7 host on Python 3.11, hpcp's root suite; main's last CI run green or each red job's cause found outside the pinned code). Per the main session's notes the gate passed for TSVZ 5.17, multiCMD 1.64, statblk 1.61 and statbtrfs 0.44 (2026-11-13), and for hpcp 9.92 except an EL7 spawn traceback that 9.88 and 9.91 have too (deferred in hpcp). Before the upload: a test that passed only on a rerun (a TSVZ conformance run on macOS, once) needs its cause written down. multiCMD 1.65 and hpcp 9.93 (both released 2026-11-13) are not pinned this week. From: the user's picks 2026-11-13, "Quick hpcp 9.92 first (Recommended)" and "Pin 9.92 (Recommended)".
- **Version, date, release row and lock file.** State: not started. Where: pyproject.toml `version = "2026.46.1"` to `"2026.47.1"`; README "Current version" and "Released" (2026-11-16) and a new first row in "Recent releases" for the tools that changed (as 5ce69bd did for week 46); uv.lock refreshed. From: README cadence.
- **Release 2026.47.1.** State: not started; follows the three rows above. Next: review of the release before any upload, `uvpack` at the repo root, push master and the tag v2026.47.1 (public repo: the user's release OK covers them), `gh release create` listing the tools that changed that week. From: README cadence.

### Week 48 roll-up (Monday 2026-11-23; no written plan)
- **Pins of the tools released after the week-47 gate.** State: not started. Where: multiCMD 1.65 (tag v1.65, released 2026-11-13; shipped to PyPI to ride this roll-up), hpcp 9.93 (tag v9.93, released 2026-11-13), TSVZ 5.18 and multiCMD 1.66 (tags v5.18 and v1.66, released 2026-11-14), and whatever else is tagged by then. Each passes the full gate first; multiCMD moves only beside versions of hpcp, statblk and statbtrfs tested with it (the README line above). From: the main session's call (2026-11-13).

### Standing rows
- **Deferred work** is in `docs/limitations.md` (entries of kind "deferred"); each names when it is revisited. There are none now: its four entries are known limitations.

## Questions waiting for the user
- None.

## Closed
- (none yet)
