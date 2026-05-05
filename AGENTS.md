# AGENTS.md

## Cursor Cloud specific instructions

This is an **embedded firmware** project for the Teknic ClearCore motion controller (ARM Cortex-M4).
There is no web server, database, or software service to run — the only dev workflow is **compile** and **lint**.

### Build

The sketch directory name must match the `.ino` filename. When compiling from the repo root, copy (or symlink) files into a directory named `LPM/`:

```bash
mkdir -p /tmp/LPM
cp LPM.ino LPM_*.cpp LPM_*.h /tmp/LPM/
arduino-cli compile --fqbn ClearCore:sam:clearcore /tmp/LPM
```

FQBN: `ClearCore:sam:clearcore` (lowercase `clearcore`).

### Lint

```bash
cppcheck --enable=all --suppress=missingInclude --suppress=unusedFunction --suppress=unmatchedSuppression --language=c++ LPM_*.cpp LPM_*.h
```

System-header `missingIncludeSystem` warnings are expected (ClearCore/Arduino headers aren't on the host include path).

### Key gotchas

- **No tests**: the repo has no unit test framework; verification is compile + lint only.
- **No runtime**: firmware cannot be executed or uploaded without physical ClearCore hardware. Build artifacts (`.bin`, `.elf`) are cross-compiled for ARM, not runnable on x86.
- **SD library**: the `SD` Arduino library must be installed separately (`arduino-cli lib install SD`).
