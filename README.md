# Tidal Cycles Setup

Personal live coding environment built on [Tidal Cycles](https://tidalcycles.org/) and [SuperCollider](https://supercollider.github.io/).

## System

- OS: Xubuntu 24.04 LTS
- SuperCollider 3.13.0
- SuperDirt (installed from source)
- sc3-plugins 3.13.0 (built from source)
- Tidal Cycles 1.10.1
- Editor: VS Code with Tidal Cycles extension

## Repository contents

| File | Description |
|---|---|
| `BootTidal.hs` | Tidal boot configuration |
| `test.tidal` | Practice patterns |
| `pattern01.tidal` | Saved pattern sketches |
| `tidal-operating-manual.md` | Full setup and usage reference |
| `.gitignore` | Excludes audio files and build artifacts |

## Starting a session

1. Open SCIDE and press `Ctrl+B` to boot the server
2. Paste and run the SuperDirt block from the operating manual
3. Press `Ctrl+Shift+L` to recompile the class library (required for sc3-plugins)
4. Reboot server with `Ctrl+B` and re-run the SuperDirt block
5. Open VS Code, open a `.tidal` file, press `Shift+Enter` on any pattern line

See `tidal-operating-manual.md` for the full SuperDirt boot block, troubleshooting, and pattern reference.
