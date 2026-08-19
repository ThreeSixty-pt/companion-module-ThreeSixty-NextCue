# NextCue

Bitfocus Companion module for **NextCue** by [ThreeSixty.pt](https://threesixty.pt).

Everything NextCue can be told to do from a show controller is here: transport,
cue navigation, time changes, outputs, overlay messages and the rundown. Drag a
preset onto a page and it arrives with its icon, colour and feedback already set.

## Configuration

| Field | Default | Description |
|---|---|---|
| **NextCue host** | `127.0.0.1` | IP address of the PC running NextCue |
| **TCP port** | `3000` | Settings → Network / API → Companion TCP, inside NextCue |

Enable **Settings → Network / API → Companion TCP** in NextCue and use the same
port on both sides. The connection carries commands out and a status broadcast
back at 10 Hz, which is what drives the variables and feedbacks below.

---

## Actions

### Transport

| Action | Sends | What it does |
|---|---|---|
| Start | `Start` | Starts the cue that is loaded |
| Pause / Resume (toggle) | `Pause` | Pauses when running, resumes when paused |
| Resume | `Resume` | Resumes a paused timer only |
| Stop | `Stop` | Stops and holds the value it stopped at |
| Reset | `Reset` | Returns the cue to its full duration — this is the one that rewinds |
| Restart current cue | `Restart` | Stop, reload, start again from the top |
| Run 10 second test | `TestMode` | Adds a ten second test cue and starts it |

### Cue navigation

| Action | Sends | What it does |
|---|---|---|
| Take next cue | `Next` | Stops the current cue and starts the next one |
| Take previous cue | `Previous` | Stops the current cue and starts the previous one |
| Fire (start current) | `FireNext` | Starts the loaded cue without moving the selection |
| Cue next (stand by) | `CueNext` | Moves the selection forward without starting |
| Cue previous (stand by) | `Undo` | Moves the selection back without starting |
| Reload current cue | `CueCurrent` | Reloads the cue so the display returns to its duration |
| Fire cue by ID | `FireTimerWithID#<id>` | Loads that cue and starts it |
| Load cue by ID | `CueTimerWithID#<id>` | Loads that cue, stands by |
| Fire cue by position | `FireCueIndex#<n>` | Position in the list, 1 = first |
| Load cue by position | `CueCueIndex#<n>` | Position in the list, stands by |

### Time

| Action | Sends | What it does |
|---|---|---|
| Add 1 minute | `AddMinute` | Adds a minute to the timer on air |
| Subtract 1 minute | `SubMinute` | Takes a minute off |
| Add X minutes | `AddXMinutes#<n>` | Any number of minutes |
| Subtract X minutes | `SubXMinutes#<n>` | Any number of minutes |
| Set cue duration | `SetDuration#HH:MM:SS` | Writes a new duration onto the cue — a lasting change |
| Count down to a clock time | `InitEndTimeTimer#HH:MM:SS` | Sets the cue duration so it reaches zero at that wall-clock time. It cues it up — press START to run it |

### Outputs

| Action | Sends | What it does |
|---|---|---|
| Fullscreen output | `Fullscreen#on\|off\|toggle` | The audience display window |
| NDI output | `NDI#on\|off\|toggle` | The NDI sender |
| Blackout | `Blackout#on\|off\|toggle` | Blacks out every output at once |
| Lock the operator window | `Lock#on\|off\|toggle` | Stops a stray click on the show machine changing anything. **A locked NextCue also ignores transport commands from Companion** — put the Lock preset on a button so you can see it |

### Messages

| Action | Sends | What it does |
|---|---|---|
| Show overlay message | `Message#<text>` | Puts a line over the countdown on every output |
| Clear overlay message | `Message#` | Removes it |
| Show / hide message | `ToggleMessage` | Flips visibility, keeping the text |
| Recall saved message | `SavedMessage#<n>` | Position in the saved-messages library, 1 = first |

### Rundown

| Action | Sends | What it does |
|---|---|---|
| Move selection up | `MoveNextUp` | Standing-by selection up one |
| Move selection down | `MoveNextDown` | Standing-by selection down one |
| Reload rundown | `InitList` | Reloads the rundown from disk |

### Advanced

**Send raw command** takes any verb and parameter, for anything added to NextCue
after this version of the module.

---

## Feedbacks

| Feedback | True when |
|---|---|
| Fullscreen is on | The audience display window is open |
| NDI is on | NextCue is sending NDI |
| Timer is paused | The timer is held |
| Blackout is on | Outputs are blacked out |
| Overlay message visible | A message is on the outputs |
| Operator lock is on | NextCue is locked and ignoring transport commands |
| **Timer colour (live)** | Advanced — paints the button in NextCue's own current state colour, so the Stream Deck turns amber and red at the same moment the stage display does |

The lock feedback needs NextCue 1.5.1 or newer; older versions do not report the
lock in their status broadcast.

---

## Variables

Time: `time_full`, `time_hours`, `time_minutes`, `time_seconds` (all signed — they
go negative in overrun), plus `hours` / `minutes` / `seconds` aliases.

Cue: `cue_name`, `next_cue_name`, `next_cue_duration`, `end_time`,
`schedule_offset`, `speed`.

Colour: `fg_color`, `bg_color` as `#RRGGBB`.

State: `is_fullscreen`, `is_ndi`, `is_paused`, `is_blackout`, `is_message`, `is_locked`,
`is_preview`, `is_presenter`, `is_stm`, `is_clock`.

---

## Presets

Seven categories, ready to drag onto a page: **Transport**, **Cue navigation**,
**Time**, **Outputs**, **Messages**, **Rundown** and **Displays** (readouts that
show the timer, the current cue, the next cue and the clock time the cue ends).

The icons are generated by `tools/make-icons.py` into `companion/icons/`. Re-run
that script to restyle the whole set at once.

---

## Not controllable from Companion

By design, since they open windows or dialogs on the show machine: settings,
diagnostics, the file dialogs (open/save rundown, import/export settings), the
remote-control and share QR dialogs, and editing cue properties other than
duration. Use the operator window for those.

Support: support@threesixty.pt
