# cpugraphapplet

A tiny [Waybar](https://github.com/Alexays/Waybar) custom module that shows
average CPU usage over the last 16 seconds as a Unicode sparkline
(`▁▂▃▄▅▆▇█`) directly in the bar.

- Single Python file, standard library only — no `pip install`, no venv.
- One averaged graph across all cores (remains usable even with a large number of cores).
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

- **State file:** `$XDG_RUNTIME_DIR/cpugraphapplet.state` (falls back to
  `/tmp`). Wiped on reboot, which is what you want.
- **First 16 seconds after start:** the graph fills in from the right as
  samples accumulate. Width stays fixed at 16 characters so the bar never
  jumps.
- **Requirements:** Python 3 (any modern version) and a font that renders
  U+2581–U+2588. Most Nerd Fonts and DejaVu Sans Mono are fine.
