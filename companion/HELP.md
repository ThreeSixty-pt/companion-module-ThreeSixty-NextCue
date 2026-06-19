# NextCue

Bitfocus Companion module for **NextCue** by [ThreeSixty.pt](https://threesixty.pt).

## Configuration

| Field | Default | Description |
|---|---|---|
| **NextCue host** | `127.0.0.1` | IP address of the PC running NextCue |
| **TCP port** | `3000` | Companion TCP port (Settings → Network → Companion TCP in NextCue) |

Make sure **Settings → Network → Companion TCP** is enabled inside NextCue and the same port number is set on both sides.

## Actions

### Transport
| Action | Notes |
|---|---|
| Start | Start the currently loaded cue |
| Fire Next | Start current cue without advancing |
| Cue Next | Advance selection without firing |
| Reload current cue | Stop and reload the current cue |
| Fire timer with ID | Start a specific cue by its ID (parameter) |
| Load cue with ID | Load a specific cue without firing (parameter) |
| Pause / Resume | Toggles |
| Restart current cue | Stop and restart from the beginning |
| Undo | Take previous cue |

### Time adjustments
- **+1 / −1 minute** — quick nudges
- **+X / −X minutes** — custom amount (supports variables)
- **Set duration** — `HH:MM:SS` (supports variables)

### Output
- **Fullscreen output** — On / Off / Toggle
- **NDI output (toggle)**
- **Blackout** — On / Off / Toggle

### Overlay
- **Set overlay message** — supports variables, leave empty to clear
- **Clear overlay message**

### Rundown navigation
- **Move selection up / down**
- **Reload rundown**

## Variables

The module exposes everything NextCue broadcasts:

- `$(NextCue:time_full)` — `HH:MM:SS` signed (`-` prefix when in overrun)
- `$(NextCue:time_hours)`, `$(NextCue:time_minutes)`, `$(NextCue:time_seconds)`
- `$(NextCue:cue_name)`, `$(NextCue:next_cue_name)`, `$(NextCue:end_time)`
- `$(NextCue:speed)`, `$(NextCue:fg_color)`, `$(NextCue:bg_color)`
- Booleans: `$(NextCue:is_fullscreen)`, `is_ndi`, `is_paused`, `is_blackout`, `is_message`

## Feedbacks

Use these to make buttons change colour based on NextCue's state:

- **Fullscreen is on**
- **NDI is on**
- **Timer is paused**
- **Blackout is on**
- **Overlay message visible**
- **Timer colour (live)** — tints the button with NextCue's current timer colour (Normal/Warning/Critical/Overrun)

## Presets

A full Transport / Time / Output / Display preset library is included — drag the ready-made buttons straight into your grid. The *Live timer* preset shows the actual countdown text with the live colour as a feedback.

## Troubleshooting

- **"Bad config — host is empty"** → fill in NextCue's IP
- **Connecting forever** → check NextCue's Companion TCP is enabled, the port matches, and Windows Firewall allows TCP 3000 on the NextCue machine (the NextCue installer adds this rule automatically)
- **Buttons go grey when NextCue restarts** → connection auto-reconnects within a few seconds
