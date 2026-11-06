# Limitations

Approved known limitations and deferrals. Reviewers: read this first, and don't raise these again.

### pip 9 cannot upgrade from or uninstall 2026.45.3 and earlier
- Kind: known limitation
- What the operator sees: on stock EL7 and EL8 pip 9.0.3, `pip3 install -U cas-toolbox` from 2026.45.3 or earlier, and `pip3 uninstall cas-toolbox` of those releases, fail with `configparser.DuplicateOptionError: ... option 'multicmd' in section 'console_scripts' already exists`. The installed release keeps working. `pip3 install -U pip` once clears it, measured on EL7 and EL8.
- Why: pip 9 parses the installed release's `entry_points.txt` with a parser that folds case, and those releases list `multiCMD` and `multicmd`, `multiSSH3` and `multissh3`, `TSVZ` and `tsvz`. Nothing in a new release can change the old metadata.
- Revisit when: EL's stock pip stops being 9, or never.
- Approved: 2026-11-06, the user chose "Known limitation (Recommended)".
