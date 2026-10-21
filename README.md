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
host and it runs, no packaging required. (`multiSSH3`'s optional shell
autocomplete is the one exception: it needs `mssh_complete.py` next to
`multiSSH3.py`.)

## Project metadata

| | |
|---|---|
| **Current version** | `2026.43.1` |
| **Released** | 2026-10-21 |
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
| [hpcp](https://github.com/yufei-pan/hpcp) | `hpcp.py` | [`hpcp`](https://pypi.org/project/hpcp/) | 9.78 | `hpcp` | Highly-parallel `cp` / imaging for HPC storage |
| [multiCMD](https://github.com/yufei-pan/multiCMD) | `multiCMD.py` | [`multicmd`](https://pypi.org/project/multiCMD/) | 1.54 | `mcmd`, `multicmd`, `multiCMD` | Run many subprocesses in parallel (base library) |
| [multiSSH3](https://github.com/yufei-pan/multiSSH3) | `multiSSH3.py` | [`multissh3`](https://pypi.org/project/multissh3/) | 6.37 | `mssh`, `mssh3`, `multissh`, `multissh3`, `multiSSH3`, `mssh-complete` | Parallel SSH across hosts, curses TUI |
| [simple-iotest](https://github.com/yufei-pan/simple-iotest) | `iotest.py` | [`simple-iotest`](https://pypi.org/project/simple-iotest/) | 3.70.0 | `iotest`, `simple-iotest` | Dead-simple filesystem I/O benchmark |
| [statbtrfs](https://github.com/yufei-pan/statbtrfs) | `statbtrfs.py` | [`statbtrfs`](https://pypi.org/project/statbtrfs/) | 0.36 | `statbtrfs` | Btrfs status / scrub orchestration |
| [Tee_Logger](https://github.com/yufei-pan/Tee_Logger) | `src/Tee_Logger.py` | [`tee-logger`](https://pypi.org/project/tee-logger/) | 6.42 | *(library only)* | Logger that tees to dated files + stdout |
| [TSVZ](https://github.com/tsvz-org/TSVZ) | `TSVZ.py` | [`tsvz`](https://pypi.org/project/tsvz/) | 4.9 | `tsvz`, `TSVZ` | Key-value store / in-memory DB backed by a TSV file |
| [statblk](https://github.com/yufei-pan/statblk) | `statblk.py` | [`statblk`](https://pypi.org/project/statblk/) | 1.55 | `statblk` | Block-device / partition info table |
| [firewatcher](https://github.com/yufei-pan/firewatcher) | `firewatcher.py` | [`firewatcher`](https://pypi.org/project/firewatcher/) | 1.60 | `firewatcher` | Journal/log incident capture with off-box context |

Internal dependency graph worth knowing when a pin moves:

- `multiCMD` is the foundation — `hpcp`, `statbtrfs` and `statblk` all depend on it.
- `simple-iotest` depends on `tee-logger`.

## Recent releases

| Version | Date | What changed |
|---|---|---|
| `2026.43.1` | 2026-10-21 | **`multissh3` 6.31 → 6.37.** **Breaking:** mssh no longer takes an abbreviation of one of its options (`--user` for `--username`, `-c` for `-ci`); spell them in full. Hosts skipped as recently unreachable now fail the run (hosts excluded with `-X` do not), and targets that name no hosts are an error. The word after `-ea` and `-ia` is their value also when it starts with `-` (`-ea -v`); a bare ssh or ipmitool option that needs its own value there (`-ea -p 2222`, quotes forgotten), or an mssh password option, still stops mssh. **Safety, and it needs a reinstall:** in a shell with `nocasematch` on (or after a completion file turned it on), bash's completion read `complete -f cmd` as `-F`, and TAB ran the remote command locally (6.31–6.36). The shell glue is a copy installed per user account, so for every account on every host where `mssh --completion install` was run, **run it again as that account**: until then the old glue, and the bug, stay. With bash's `completion-ignore-case` on, completion now matches the word in any case, as the shell's locale lowers letters; bash's completion gives the shell its `nocasematch` back after a Ctrl-C, keeps its variables from the shell's own completion functions, and works with any `IFS`. ssh can no longer prompt behind the curses view or show a typed password; `ConnectTimeout` 30 s and keepalives are the default (your ssh config wins), so one hung host no longer stops a run. File copies: `-ea`'s ssh options no longer reach rsync as files (a private key passed with `-i` was copied to the hosts). Passwords reach sshpass and ipmitool through their environment, not their command line; a `-p` password is not echoed, and the `> mssh` line and the history say `-p -` (`MSSH_PASSWORD` gives one; `--store_config_file` still writes it to the config file). `--ipmi` tries admin/admin only when no other login is configured. `root@dbs` with an alias whose members carry their own user no longer logs in as `root@postgres@db1`. The TUI no longer polls (1000 waiting hosts: 0.13 cores instead of 1.47), and output lines that hosts print alike are kept once (200 hosts × 20,000 identical lines with `-P`: 3.0 GB → 0.17 GB). A bare `mssh` with `DEFAULT_HOSTS` null no longer crashes (it means all hosts), and with `$XDG_RUNTIME_DIR` set, mssh reads its unreachable-host state where it writes it. **`hpcp` 9.73 → 9.78:** `hpcp -rm /` is refused (9.73 hung on it). Plain `-rm` and `-rme` remove by directory descriptor, so a directory swapped for a symlink in a tree others can write to no longer leads them outside it. Legacy `-fl -rme` with a bare relative destination (`data`, `.`) no longer builds its extras under `/` (9.73 removed `/data/extra`). `-rme` leaves a filesystem mounted below the destination alone with its files, as `rsync -x` does: bind mounts, btrfs subvolumes, and automounts and NFSv4 submounts mounted on entering a directory. Trees deeper than Python's recursion limit are listed, copied and removed. On Python 3.6, `-rm -pfl -F`, non-UTF-8 file names and `-rme`'s automount checks work. `-dd` no longer leaves `/hpcp-ext-feature-probe` in `/`: e2fsprogs 1.45 and 1.46 (EL8, EL9, Ubuntu 20.04 and 22.04, Debian 11) created it when 9.61–9.77 probed mke2fs as root, so delete it there. The probe reads mke2fs in the C locale: a translated mke2fs made every feature count as known. **`tsvz` 4.7 → 4.9:** a read is redone when another tool rewrites a part in place under it (a scrub), and a read still changing after three tries is incomplete: `get` and `read` exit 1 and the handler refuses `pop` and `setdefault`. Reads start at the last snapshot part (`#_snapshot_#`, spec §19.10). `scrub` refuses a store declaring a spec version newer than it implements and writes nothing; so does `clear` on a single-part store (on a multi-part one it still appends tombstones). A marker value ending in a newline (`0<LF>`) no longer crashes the read. **`simple-iotest` 3.69.1 → 3.70.0:** new `--json` summary, `-bs/--block_size`, `--verify` (a CRC-32 trailer checked on every read), `meta` mode (create, stat and unlink of empty files), `--start_at` for synchronized multi-host starts, and an opt-in per-operation timeline (`-tl`). Fixed: lost results on long `-d` paths, the report printed twice, and misleading cache-drop and sync figures; invalid sizes exit 2. **`multicmd` 1.53 → 1.54**, with **`statblk` 1.54 → 1.55** and **`statbtrfs` 0.35 → 0.36** embedding it: the terminal-size fallback reads the columns, not the rows, so `statblk | less` no longer squeezes the table to the terminal's height. These three, **`firewatcher` 1.59 → 1.60**, `multissh3`, `tsvz` and `simple-iotest` now ship the GPL-3.0 LICENSE in their sdist and wheel (`tee-logger` 6.42 does not yet). |
| `2026.42.2` | 2026-10-16 | **Hotfix: `hpcp` 9.72 → 9.73.** `-rm` with a destination (a move) no longer deletes sources it did not copy: it is skipped after any copy error (`Remove skipped`, 212), and otherwise removes only the files and links the copy listed, then empty directories, so files written during the move, FIFOs, sockets, device nodes and the files of a `-do` copy stay; a move onto itself (`hpcp -rm /data/x -d /data/`) is refused, and `-rm -di NEW.img` creates the image first (9.72 removed the sources and made no image). The move's removal goes by directory descriptors without following symlinks, so a source or directory swapped for a symlink during the copy is not removed from, and it needs about half of 9.72's metadata operations. `-rme -di` no longer empties the image it just filled; `-rme` keeps the copy of an excluded path (absolute or not, `-e 'cache/'` included) and a filesystem's `lost+found`; a `-d` glob no longer yields `/`. One unreadable file or directory no longer stops a copy (`Listing error`, 209; the rest is copied), and `-diff` refuses a partial listing instead of writing delete rows. Link tracking is opt-in (`-lt`; it had been inactive since May 2025). `-dd` mounts source partitions read-only unless they are mounted live, and xfs clones mount with `nouuid`. Pool workers get the run's settings (`-co`, `-ncd`, `-hs`, `-ctl` on macOS, Windows and Python 3.14+). |
| `2026.42.1` | 2026-10-13 | **`tsvz` 3.39 → 4.7 (major):** `.tsvz` / `.csvz` / `.nsvz` / `.psvz` files now follow tsvz-spec-v1, so a file 3.39 wrote under one of those names reads differently — rename it to `.tsv` (see TSVZ's *Breaking changes from 3.39*); `.tsv` / `.csv` get bug fixes only. 4.7 passes the whole spec conformance suite: carriage returns and zero bytes in values are written as `<CR>` and `<NUL>` (older versions read those as text, so stop older `tsvz serve` handlers before upgrading; a value 4.2/4.3 wrote with a raw zero byte now reads as damage), `parts` names codecs, and a part in a codec this Python lacks (zstd before 3.14) is refused. Damaged compressed members and zero-filled lines are recovered, skipped or stop the read as the store's `#_on_corruption_#` says (recover by default); appends never read the store; a key ending in whitespace can be read and deleted by name, so `tsvz keys s | tsvz delete s -` deletes every key; tables line up CJK text. `multissh3` 6.25 → 6.31: config and env files are only trusted when only you (or root) can change them and are no longer read from the current directory (a host name defined only there now says so), stdin is closed when no TUI runs, faster output reading and host lookups, `~/.ssh/config` aliases and IPv6-only hosts, one Ctrl-C cancels the run, the summary table lines up CJK and colored output, and piped output (`mssh ... | less`) is sized by the terminal's width instead of its height. New opt-in shell autocomplete for bash, zsh and fish (`mssh --completion install`, new `mssh-complete` command): TAB completion of hosts, options and commands from history and every host source, grey suggestions, a Ctrl-R picker, and warnings before Enter about unknown or unreachable hosts and conflicting options, from a background prober that only uses DNS, `ssh -G` (which runs the ssh config's `Match exec` commands) and ping. `multicmd` 1.49 → 1.53: real exit status, `-t` no longer kills a command after 60 s by default (0 = never is the new default; pass `-t 60` to keep the old behavior), commands from files or stdin, tables line up CJK text. `statblk` 1.42 → 1.54: `--check` health mode with exit codes, spun-down drives are not woken for SMART, CJK names and labels line up in the table. `statbtrfs` 0.30 → 0.35: `-w/--watch`, btrfs-progs 6.x missing devices, CJK paths line up in the table. `simple-iotest` 3.68.0 → 3.69.1: failed I/O ops are counted and make the exit code 1 (was always 0), files of 2 GiB and up are written and read in full, a run that won't fit on disk is refused before anything is written, new `-u/--unique_files` (dedup-proof test data) and `-cl/--cleanup`, `-addr` no longer needs `curl`, and read/index runs need half or none of the data memory. `tee-logger` 6.40 → 6.42: hang, crash-data-loss and log-cleanup fixes; the `Log file:` banner moves from stdout to stderr, compressed logs (as `iotest` writes them) use zstd instead of xz on Python 3.14+, and an unwritable log directory falls back to `$TMPDIR/teeLogger-<uid>/`. `hpcp` 9.64 → 9.72 (embeds multiCMD 1.52): disk-level patch images (`-dd -diff` makes and `-siff` applies a patch of whole GPT disks: changed files per filesystem, raw areas, identity values), a repair step and exact-case names on apply, exclusive opens and a disk lock while writing, labels / GPT names / exFAT serials copied exactly, `-e` matched inside each mounted filesystem, automounted ESPs and btrfs subvolume roots handled, forced removals that cannot leave the target, and a fix for `-diff /` with a blank line in its `-x` file (or `-e /`), which wrote a patch deleting every file: such patterns are now refused. `firewatcher` 1.58 → 1.59 (less CPU and memory per line). |
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
| `iotest` / `simple-iotest` | simple-iotest |
| `statbtrfs` | statbtrfs |
| `tsvz` / `TSVZ` | TSVZ |
| `statblk` | statblk |
| `firewatcher` | firewatcher |

Every command supports `--help` / `-h` and `-V` / `--version`, except
`mssh-complete`, the engine the shell glue runs (run it bare for its usage).

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
- [TSVZ](https://github.com/tsvz-org/TSVZ)
- [statblk](https://github.com/yufei-pan/statblk)
- [firewatcher](https://github.com/yufei-pan/firewatcher)
