# Tidal Cycles + SuperCollider Operating Manual

## System Overview

- **SuperCollider / SCIDE** — audio server and synthesis engine
- **SuperDirt** — audio engine that receives pattern messages from Tidal
- **Tidal Cycles** — live coding pattern language (runs in VS Code)
- **VS Code** — editor for writing and evaluating Tidal patterns
- **Claude Code** — AI assistant integrated into VS Code for code help, file editing, and debugging

### Installed VS Code Extensions
- **Tidal Cycles** by tidalcycles — evaluates Tidal patterns from VS Code
- **vscode-supercollider** by jatinchowdhury18 — SuperCollider language support
- **Haskell** by Haskell — Haskell language support via GHCup
- **Claude Code** by Anthropic — AI coding assistant with file read/write and terminal access

---

## Starting a Session (do this every time)

### Step 1 — Open SCIDE

Launch SuperCollider IDE from your terminal:

```
scide
```

### Step 2 — Boot the audio server

Press `Ctrl+B` inside SCIDE and wait for the post window to say:

```
SuperCollider 3 server ready.
```

If sound comes out of headphones instead of speakers, unplug headphones first, then press `Ctrl+B`.

### Step 3 — Start SuperDirt

In the SCIDE editor, paste and run this block with `Ctrl+Enter`:

```supercollider
(
s.options.numBuffers = 1024 * 256;
s.options.memSize = 8192 * 16;
s.options.numWireBufs = 64;
s.options.maxNodes = 1024 * 32;

s.waitForBoot {
    ~dirt = SuperDirt(2, s);
    ~dirt.loadSoundFiles;
    s.sync;
    ~dirt.start(57120, [0, 0]);
    "SuperDirt ready".postln;
};
)
```

Wait for the post window to say `SuperDirt ready`.

### Step 4 — Open VS Code

Open VS Code and navigate to your tidal folder (`~/tidal`). Open a `.tidal` file.

### Step 5 — Start Tidal

Place your cursor on any pattern line and press `Shift+Enter`. Tidal will boot automatically and send the pattern to SuperDirt.

### Step 6 — Open Claude Code (optional)

Open the Claude Code terminal with `Ctrl+Shift+P` → "Claude Code: Open in Terminal". Run it with:

```
~/.npm-global/bin/claude
```

Use Claude Code to get help with patterns, debug errors, or edit files directly.

---

## Stopping Sound

### Stop all patterns immediately
```haskell
hush
```

### Stop a specific channel
```haskell
d1 silence
```

### Stop the SuperCollider server
In SCIDE, run:
```supercollider
Server.killAll;
```

---

## Tidal Pattern Basics

### Simple patterns

```haskell
-- Kick and snare
d1 $ sound "bd sn"

-- Hi-hats
d2 $ sound "hh hh hh hh"

-- Melodic sample
d3 $ sound "arpy arpy:2 arpy:4 arpy:7"
```

### Rests and rhythm

```haskell
-- Tilde is a rest
d1 $ sound "bd ~ sn ~"

-- Subdivide a beat with square brackets
d1 $ sound "bd [sn sn] hh ~"
```

### Speed and pitch

```haskell
-- Play at double speed
d1 $ sound "bd sn" # speed 2

-- Play a sample at a different pitch
d1 $ sound "arpy" # n 4
```

### Effects

```haskell
-- Reverb (0 to 1)
d1 $ sound "bd sn" # room 0.5 # sz 0.8

-- Delay
d1 $ sound "bd sn" # delay 0.5 # delayt 0.25 # delayfb 0.4

-- Low-pass filter
d1 $ sound "bd sn" # cutoff 800 # resonance 0.2

-- Distortion
d1 $ sound "bd sn" # shape 0.5
```

### Pattern transformations

```haskell
-- Reverse a pattern
d1 $ rev $ sound "bd sn hh cp"

-- Speed up by a factor
d1 $ fast 2 $ sound "bd sn hh cp"

-- Slow down
d1 $ slow 2 $ sound "bd sn hh cp"

-- Randomise order
d1 $ shuffle 4 $ sound "bd sn hh cp"
```

### Multiple channels

```haskell
d1 $ sound "bd sn"
d2 $ sound "hh*8"
d3 $ sound "arpy:3 ~ arpy:5 ~"
```

---

## Keyboard Shortcuts

| Action | Shortcut |
|---|---|
| Evaluate line (Tidal, VS Code) | `Shift+Enter` |
| Evaluate multiline block (Tidal) | `Ctrl+Enter` |
| Boot audio server (SCIDE) | `Ctrl+B` |
| Evaluate line (SCIDE) | `Ctrl+Enter` |
| Stop all sound (SCIDE) | `Ctrl+.` |
| Stop all Tidal patterns | `Ctrl+Alt+H` |

---

## VS Code Settings Reference

Location: `~/.config/Code/User/settings.json`

```jsonc
{
    "supercollider.sclangCmd": "/usr/bin/sclang",
    "tidalcycles.ghciPath": "/home/lucas-swann/.ghcup/bin/ghci",
    "tidalcycles.bootTidalPath": "/home/lucas-swann/tidal/BootTidal.hs",
    "haskell.manageHLS": "GHCup",

    // Tidal Cycles settings
    "tidalcycles.showOutputInConsole": true,
    "tidalcycles.feedbackColor": "rgba(100,250,100,0.3)",

    // Haskell - disable HLS for .tidal files so it doesn't interfere
    "haskell.trace.server": "off",
    "[haskell]": {
        "editor.formatOnSave": false
    },

    // Editor comfort settings
    "editor.fontSize": 16,
    "editor.lineNumbers": "on",
    "editor.wordWrap": "on",
    "editor.minimap.enabled": false
}
```

---

## Troubleshooting

### No sound from Tidal
- Check that SuperDirt is running in SCIDE (post window says `SuperDirt ready`)
- Check that the audio server is booted (server meters active in SCIDE bottom bar)
- If speakers aren't working, run `Server.killAll` in SCIDE, unplug headphones if connected, then reboot with `Ctrl+B`

### Tidal won't start in VS Code
- Make sure `~/tidal/BootTidal.hs` exists
- Check that `settings.json` has all three settings:
  - `supercollider.sclangCmd`
  - `tidalcycles.ghciPath`
  - `tidalcycles.bootTidalPath`

### Audio goes to headphones instead of speakers
Run `Server.killAll` in SCIDE with headphones unplugged, then press `Ctrl+B` to reboot.

### Haskell Language Server is slow or causes problems
Set `"haskell.manageHLS": "PATH"` in settings.json and restart VS Code.

### Claude Code command not found
Run it directly with the full path:
```
~/.npm-global/bin/claude
```

---

## File Locations

| Item | Path |
|---|---|
| Tidal project folder | `~/tidal/` |
| BootTidal.hs | `~/tidal/BootTidal.hs` |
| Dirt Samples | `~/.local/share/SuperCollider/Extensions/Dirt-Samples/` |
| SuperDirt extension | `~/.local/share/SuperCollider/Extensions/SuperDirt/` |
| VS Code settings | `~/.config/Code/User/settings.json` |
| GHCup binaries | `~/.ghcup/bin/` |
| Cabal packages | `~/.cabal/` |
| Claude Code binary | `~/.npm-global/bin/claude` |
