# cpugraphapplet

A tiny [Waybar](https://github.com/Alexays/Waybar) custom module that shows
CPU usage over the last 16 seconds as a Unicode sparkline (`▁▂▃▄▅▆▇█`)
directly in the bar.

- Single Python file, standard library only — no `pip install`, no venv.
- Two display modes: average across all cores, or the single most-loaded core.
  Both remain usable even with a large number of cores.
- 8 discrete levels, plus a `class` attribute for CSS-based color changes.
- Hover tooltip shows the current numeric percentage.

## Install

```bash
mkdir -p ~/.config/waybar/scripts
cd  ~/.config/waybar/scripts
git clone git@github.com:bnegreve/cpugraphapplet.git
```

## Waybar config

Add to `~/.config/waybar/config`:

```jsonc
"custom/cpu": {
    "exec": "$HOME/.config/waybar/scripts/cpugraphapplet/cpugraphapplet",
    "interval": 1,
    "return-type": "json",
    "tooltip": true
}
```

Then reference `"custom/cpu"` in one of `modules-left`, `modules-center`, or
`modules-right`.

## Display mode

By default the graph shows the **average** usage across all cores. Pass
`--mode max` to instead show the usage of the **single most-loaded core** —
useful for spotting a single runaway process on a many-core machine, where
the average would stay near-zero.

```jsonc
"custom/cpu": {
    "exec": "$HOME/.config/waybar/scripts/cpugraphapplet/cpugraphapplet --mode max",
    "interval": 1,
    "return-type": "json",
    "tooltip": true
}
```

You can also run two modules side by side (e.g. `custom/cpu-avg` and
`custom/cpu-max`) — each mode uses its own state file, so their histories
stay independent.

## Optional styling

The module emits a `class` of `low`, `medium`, `high`, or `critical` based on
the latest sample. Add to `~/.config/waybar/style.css`:

```css
#custom-cpu.low      { color: #a6e3a1; }
#custom-cpu.medium   { color: #f9e2af; }
#custom-cpu.high     { color: #fab387; }
#custom-cpu.critical { color: #f38ba8; }
```

If the block characters look cramped, force a monospace font:

```css
#custom-cpu { font-family: "JetBrainsMono Nerd Font", monospace; }
```

## Notes

- **State file:** `$XDG_RUNTIME_DIR/cpugraphapplet.<mode>.state` (falls back
  to `/tmp`). Wiped on reboot.  jumps.
- **Requirements:** Python 3 (any modern version) and a mono font that renders
  U+2581–U+2588. Most Nerd Fonts and DejaVu Sans Mono are fine.
