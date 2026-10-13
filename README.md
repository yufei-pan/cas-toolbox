# cas-toolbox

[![PyPI version](https://img.shields.io/pypi/v/cas-toolbox.svg)](https://pypi.org/project/cas-toolbox/)
[![Python versions](https://img.shields.io/pypi/pyversions/cas-toolbox.svg)](https://pypi.org/project/cas-toolbox/)
[![License](https://img.shields.io/pypi/l/cas-toolbox.svg)](https://www.gnu.org/licenses/gpl-3.0.html)

**Cluster Automation Scripts Toolbox** — one stop shop for tool scripts used in
cluster automation operations in high performance computing.

`cas-toolbox` is a **meta-package**: it ships no code of its own. It pins exact
releases of the nine tools below and installs all of their console commands in a
single step, so a fresh host gets the whole toolbox with one `pip install`.

Every bundled tool is a **single self-contained `.py` file** — copy one onto a
host and it runs, no packaging required.

## Project metadata

| | |
|---|---|
| **Current version** | `2026.42.1` |
| **Released** | 2026-10-13 |
| **Python** | >= 3.6 |
| **Platform** | Linux (POSIX); macOS for `multiSSH3`, `simple-iotest`, `statblk`, `tee-logger`, `TSVZ` |
| **License** | GPL-3.0-or-later |
| **Author** | Yufei Pan (pan@zopyr.us) |
| **Homepage** | [github.com/yufei-pan/cas-toolbox](https://github.com/yufei-pan/cas-toolbox) |
| **PyPI** | [pypi.org/project/cas-toolbox](https://pypi.org/project/cas-toolbox/) |
| **Build backend** | setuptools (`pyproject.toml`) |
| **Bundled tools** | 9 |

## Versioning and release cadence

Versions are `YYYY.WW.hotfix` — calendar year, ISO week number, and a hotfix
counter. For example `2026.40.1` is the first release cut in week 40 of 2026.

The bundle is deliberately **semi-stable**:

- **Mondays** are the normal re-cut. Last week's accumulated upstream changes are
  rolled into one release: the `==` pins of everything that moved are updated and
  the version bumps to the new `YYYY.WW`.
- **Mid-week** releases are for **emergency fixes only**. Only the affected tool's
  pin changes and the `hotfix` segment increments (`2026.39.1` → `2026.39.2`),
  leaving the week number alone.
- Releasing an individual upstream tool is independent and can happen any day.
  Its pin bump normally rides along in the next Monday roll-up.

Upstream tools use their own `x.y` / `x.y.z` schemes; the pins in this package
record exactly which release is in the bundle.

## Included tools

Versions listed are the ones **pinned by the current release** of `cas-toolbox`.

| Tool | Module | PyPI package | Pinned | Commands | What it does |
|---|---|---|---|---|---|
| [hpcp](https://github.com/yufei-pan/hpcp) | `hpcp.py` | [`hpcp`](https://pypi.org/project/hpcp/) | 9.72 | `hpcp` | Highly-parallel `cp` / imaging for HPC storage |
| [multiCMD](https://github.com/yufei-pan/multiCMD) | `multiCMD.py` | [`multicmd`](https://pypi.org/project/multiCMD/) | 1.53 | `mcmd`, `multicmd`, `multiCMD` | Run many subprocesses in parallel (base library) |
| [multiSSH3](https://github.com/yufei-pan/multiSSH3) | `multiSSH3.py` | [`multissh3`](https://pypi.org/project/multissh3/) | 6.31 | `mssh`, `mssh3`, `multissh`, `multissh3`, `multiSSH3`, `mssh-complete` | Parallel SSH across hosts, curses TUI |
| [simple-iotest](https://github.com/yufei-pan/simple-iotest) | `iotest.py` | [`simple-iotest`](https://pypi.org/project/simple-iotest/) | 3.69.1 | `iotest` | Dead-simple filesystem I/O benchmark |
| [statbtrfs](https://github.com/yufei-pan/statbtrfs) | `statbtrfs.py` | [`statbtrfs`](https://pypi.org/project/statbtrfs/) | 0.35 | `statbtrfs` | Btrfs status / scrub orchestration |
| [Tee_Logger](https://github.com/yufei-pan/Tee_Logger) | `src/Tee_Logger.py` | [`tee-logger`](https://pypi.org/project/tee-logger/) | 6.42 | *(library only)* | Logger that tees to dated files + stdout |
| [TSVZ](https://github.com/yufei-pan/TSVZ) | `TSVZ.py` | [`tsvz`](https://pypi.org/project/tsvz/) | 4.7 | `tsvz`, `TSVZ` | Key-value store / in-memory DB backed by a TSV file |
| [statblk](https://github.com/yufei-pan/statblk) | `statblk.py` | [`statblk`](https://pypi.org/project/statblk/) | 1.54 | `statblk` | Block-device / partition info table |
| [firewatcher](https://github.com/yufei-pan/firewatcher) | `firewatcher.py` | [`firewatcher`](https://pypi.org/project/firewatcher/) | 1.59 | `firewatcher` | Journal/log incident capture with off-box context |

Internal dependency graph worth knowing when a pin moves:

- `multiCMD` is the foundation — `hpcp`, `statbtrfs` and `statblk` all depend on it.
- `simple-iotest` depends on `tee-logger`.

## Recent releases

| Version | Date | What changed |
|---|---|---|
| `2026.42.1` | 2026-10-13 | **`tsvz` 3.39 → 4.7 (major):** `.tsvz` / `.csvz` / `.nsvz` / `.psvz` files now follow tsvz-spec-v1, so a file 3.39 wrote under one of those names reads differently — rename it to `.tsv` (see TSVZ's *Breaking changes from 3.39*); `.tsv` / `.csv` get bug fixes only. 4.7 passes the whole spec conformance suite: carriage returns and zero bytes in values are written as `<CR>` and `<NUL>` (older versions read those as text, so stop older `tsvz serve` handlers before upgrading; a value 4.2/4.3 wrote with a raw zero byte now reads as damage), `parts` names codecs, and a part in a codec this Python lacks (zstd before 3.14) is refused. Damaged compressed members and zero-filled lines are recovered, skipped or stop the read as the store's `#_on_corruption_#` says (recover by default); appends never read the store; a key ending in whitespace can be read and deleted by name, so `tsvz keys s | tsvz delete s -` deletes every key; tables line up CJK text. `multissh3` 6.25 → 6.31: config and env files are only trusted when only you (or root) can change them and are no longer read from the current directory (a host name defined only there now says so), stdin is closed when no TUI runs, faster output reading and host lookups, `~/.ssh/config` aliases and IPv6-only hosts, one Ctrl-C cancels the run, the summary table lines up CJK and colored output, and piped output (`mssh ... | less`) is sized by the terminal's width instead of its height. New opt-in shell autocomplete for bash, zsh and fish (`mssh --completion install`, new `mssh-complete` command): TAB completion of hosts, options and commands from history and every host source, grey suggestions, a Ctrl-R picker, and warnings before Enter about unknown or unreachable hosts and conflicting options, from a background prober that only uses DNS, `ssh -G` (which runs the ssh config's `Match exec` commands) and ping. `multicmd` 1.49 → 1.53: real exit status, no default timeout, commands from files or stdin, tables line up CJK text. `statblk` 1.42 → 1.54: `--check` health mode with exit codes, spun-down drives are not woken for SMART, CJK names and labels line up in the table. `statbtrfs` 0.30 → 0.35: `-w/--watch`, btrfs-progs 6.x missing devices, CJK paths line up in the table. `simple-iotest` 3.68.0 → 3.69.1: failed I/O ops are counted and make the exit code 1 (was always 0), files of 2 GiB and up are written and read in full, a run that won't fit on disk is refused before anything is written, new `-u/--unique_files` (dedup-proof test data) and `-cl/--cleanup`, `-addr` no longer needs `curl`, and read/index runs need half or none of the data memory. `tee-logger` 6.40 → 6.42: hang, crash-data-loss and log-cleanup fixes. `hpcp` 9.64 → 9.72 (embeds multiCMD 1.52): disk-level patch images (`-dd -diff` makes and `-siff` applies a patch of whole GPT disks: changed files per filesystem, raw areas, identity values), a repair step and exact-case names on apply, exclusive opens and a disk lock while writing, labels / GPT names / exFAT serials copied exactly, `-e` matched inside each mounted filesystem, automounted ESPs and btrfs subvolume roots handled, forced removals that cannot leave the target, and a fix for `-diff /` with a blank line in its `-x` file (or `-e /`), which wrote a patch deleting every file: such patterns are now refused, `firewatcher` 1.58 → 1.59 (less CPU and memory per line). |
| `2026.40.1` | 2026-09-28 | `simple-iotest` 3.67.0 → 3.68.0: IOPS (call/sync) and op counts in every mode's report, including index; fixed worker count for repeated role keys (e.g. `write s`), which over-reported size and call bandwidth. |
| `2026.39.2` | 2026-09-23 | Mid-week hotfix. Restored `requires-python >= 3.6` (2026.39.1 had raised it to 3.8 for TSVZ 4.0 while still pinning `tsvz` 3.39, so pip on 3.6 silently stayed on 2026.38.1). `hpcp` 9.62 → 9.64, `multissh3` 6.24 → 6.25, `firewatcher` 1.57 → 1.58. |

Full history is in the [commit log](https://github.com/yufei-pan/cas-toolbox/commits/master).

## Requirements

- Python >= 3.6

Nothing outside the standard library is required; the optional libraries below
only add features or speed.

## Optional Python libraries

Heavier libraries are imported defensively — every tool runs without them, just
with reduced functionality or performance.

| Library | Used by | Effect when present |
|---|---|---|
| `curses` | `multiSSH3` | Interactive TUI instead of the plain text output |
| `numpy` | `iotest` | Much faster random-number generation in the I/O benchmark |
| `xxhash` | `hpcp` | Faster hashing; falls back to `hashlib.blake2b` |
| `resource` | `multiSSH3`, `TSVZ` | Resource-usage reporting |
| `python-dateutil` | `tee-logger` | Parsing dated log directory names when cleaning up old logs |
| `argcomplete` | `TSVZ`, `statblk`, `statbtrfs` | Shell tab completion (`pip install '<tool>[completion]'`) |

`xxhash` is declared by `hpcp` but imported defensively, so `hpcp` still works
without it.

## Installation

Use pip:

```bash
pip install cas-toolbox
```

Use pipx:

```bash
pipx install cas-toolbox
```

Use uv (as an isolated tool, with `numpy` for the `iotest` benchmark):

```bash
uv tool install --with numpy cas-toolbox
```

Use uv to add it as a dependency:

```bash
uv add cas-toolbox
```

> **Note on `numpy`:** it is only used by `iotest`'s random-number generator and
> is not needed anywhere else in the toolbox.

To install one tool on its own, install its PyPI package directly instead of this
meta-package.

## Commands provided

| Command | Tool |
|---|---|
| `hpcp` | hpcp |
| `mcmd` / `multicmd` / `multiCMD` | multiCMD |
| `mssh` / `mssh3` / `multissh` / `multissh3` / `multiSSH3` | multiSSH3 |
| `mssh-complete` | multiSSH3 (autocomplete engine behind `mssh --completion`) |
| `iotest` | simple-iotest |
| `statbtrfs` | statbtrfs |
| `tsvz` / `TSVZ` | TSVZ |
| `statblk` | statblk |
| `firewatcher` | firewatcher |

Every command supports `--help` / `-h` and `-V` / `--version`.

## Verifying an install

```bash
hpcp -V && mcmd -V && mssh3 -V && iotest -V && statbtrfs -V \
  && TSVZ -V && statblk -V && firewatcher -V
```

`cas-toolbox` itself exposes no command — it is a dependency bundle only.

From a checkout that has the nine upstream repositories side by side,
`getCASVersions.sh` prints every tool's version at once:

```bash
./getCASVersions.sh
```

## Development

This repository holds only the meta-package (`pyproject.toml`, `uv.lock`,
`README.md`, `getCASVersions.sh`). The upstream sources live in their own
repositories — see the links below.

```bash
uv sync --frozen     # resolve the locked dependency set
uv build             # build wheel + sdist into dist/
```

Pins are exact (`==`) on purpose: the bundle is meant to be reproducible.

## Author

- Yufei Pan (pan@zopyr.us)

## License

GPL-3.0-or-later

## Links

- Project home: [github.com/yufei-pan/cas-toolbox](https://github.com/yufei-pan/cas-toolbox)
- [hpcp](https://github.com/yufei-pan/hpcp)
- [multiCMD](https://github.com/yufei-pan/multiCMD)
- [multiSSH3](https://github.com/yufei-pan/multiSSH3)
- [simple-iotest](https://github.com/yufei-pan/simple-iotest)
- [statbtrfs](https://github.com/yufei-pan/statbtrfs)
- [Tee_Logger](https://github.com/yufei-pan/Tee_Logger)
- [TSVZ](https://github.com/yufei-pan/TSVZ)
- [statblk](https://github.com/yufei-pan/statblk)
- [firewatcher](https://github.com/yufei-pan/firewatcher)
