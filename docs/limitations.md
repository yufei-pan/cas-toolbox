# Limitations

Approved known limitations and deferrals. Reviewers: read this first, and don't raise these again.

### pip 9 cannot upgrade from or uninstall 2026.45.3 and earlier
- Kind: known limitation
- What the operator sees: upgrading from 2026.45.3 or earlier with pip 9, the pip of EL7 and EL8, stops with `configparser.DuplicateOptionError: ... option 'multicmd' in section 'console_scripts' already exists`, with or without `--user`; the name can also be `multissh3` or `tsvz`, and can change from run to run. `pip3 uninstall cas-toolbox` of those releases stops the same way, always naming `multicmd`. The installed release keeps working. `pip3 install -U pip` once (`--user` for a user install) clears it, measured on EL7 and EL8.
- Why: pip 9 parses the installed release's `entry_points.txt` with a parser that folds case, and those releases list `multiCMD` and `multicmd`, `multiSSH3` and `multissh3`, `TSVZ` and `tsvz`. Nothing in a new release can change the old metadata.
- Revisit when: EL's stock pip stops being 9, or never.
- Approved: 2026-11-06, the user chose "Known limitation (Recommended)".

### Mixed-case commands in a venv with a space in its path
- Kind: known limitation
- What the operator sees: With pip 10 or later, in a venv whose path has a space or is too long for the kernel's `#!` line, `multiCMD`, `multiSSH3` and `TSVZ` do not start (the error varies: "bad interpreter", `import: command not found`): use `multicmd`, `multissh3` and `tsvz`. With pip 9 no command starts in such a venv, as in earlier releases. uv and system or `--user` installs are not affected.
- Why: pip writes a plain `#!<python>` line into a script file and a `/bin/sh` wrapper only for an entry point. A launcher that delegates to the lowercase twin overwrites it on a case-insensitive disk under uv. One that execs `mcmd` or `mssh` works, but prints that name for `-V`, breaks `python3 $(which multiCMD)`, and TSVZ has no such sibling.
- Revisit when: someone hits it.
- Approved: 2026-11-06, the user chose "Known limitation (Recommended)".

### `pip3 uninstall cas-toolbox` removes the commands and leaves the tools
- Kind: known limitation
- What the operator sees: `pip3 uninstall cas-toolbox` removes all 17 commands and leaves the nine tools installed. A plain `pip install multiCMD` then does nothing; `pip3 install -U --force-reinstall --no-deps multiCMD`, and the same for each other tool, brings its commands back. 2026.45.3 does the same.
- Why: the toolbox owns those command files. Without that, the upgrade from 2026.45.3 left 0 of 17 commands, and `uv tool install` and `pipx install` refused the package.
- Revisit when: the toolbox stops owning commands.
- Approved: 2026-11-06, the user chose "Known limitation (Recommended)".
