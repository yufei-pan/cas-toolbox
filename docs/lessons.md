# Lessons

One line per change to an API, format or agreed behaviour; the full story of each is in `docs/lessons-full.md`.

- **2026-11-06 cas-toolbox: the mixed-case commands are script files, and the toolbox keeps owning all 17**: pip 9 (stock on EL7 and EL8) reads `entry_points.txt` case-folded, so two commands that differ only in case stop an upgrade or an uninstall. Dropping the commands instead left 0 of 17 after an upgrade, because the old release's RECORD owns them, and uv and pipx refuse a package with none.
