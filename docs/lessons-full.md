# Lessons, in full

## 2026-11-06 cas-toolbox: the mixed-case commands are script files, and the toolbox keeps owning all 17
- Changed: `multiCMD`, `multiSSH3` and `TSVZ` are no longer entry points of cas-toolbox: they are launcher files in `scripts/`, shipped with `[tool.setuptools] script-files`. The 14 other commands stay entry points.
- Why we had to change: pip 9.0.3, the pip of EL7 and EL8, reads `entry_points.txt` with a case-folding config parser, so `pip3 install -U cas-toolbox` and `pip3 uninstall cas-toolbox` stopped with `configparser.DuplicateOptionError`.
- Original design was wrong because: it took a command name for a file name only, and not for the key of a config file that a different tool reads on uninstall.
- Lesson: a meta-package that once owned commands can't simply drop them. An upgrade removes what the old release's RECORD lists after the dependencies installed theirs, which left 0 of 17 commands, and uv and pipx refuse a package with no commands.
