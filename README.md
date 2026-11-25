# Orbit Looper

A browser looper where every loop is a circle. A circle's size is its length, so a loop twice as long is twice as wide and every playhead travels its rim at the same speed. Add new circles as ratios of existing ones (×1/2, ×3/2, ×4/3, or any p : q), and they stay locked in sync.

It's a single `index.html` with no build step and no dependencies.

## Run it

- **On the web:** open the GitHub Pages address for this repo.
- **Locally:** open `index.html` in Chrome, Edge or Firefox. If the browser won't record from a local file, serve the folder instead:

  ```sh
  python3 -m http.server
  # then open http://localhost:8000
  ```

Browsers only allow microphone recording over https or on localhost.

## Use it

- **Record new loop:** press once to start and again to stop. The length you play becomes the loop's length.
- **Add empty loop:** make a loop from a tempo and a number of beats, then record into it.
- **Load audio file:** turn a file into a loop, or load one into the selected loop (it's trimmed to fit).
- **Add a linked loop:** select a circle and pick a ratio. The new loop has the parent's length times the ratio and starts at the same moment. The panel shows when they line up again, for example "2 of these = 3 of A".
- **Record into a circle:** takes start at the top of the next cycle and last exactly one cycle. Turn on Overdub to layer a take over what's already there.
- **Mix:** mute, volume, and beats per cycle (shown as ticks on the circle) for each loop, plus an optional click on the selected loop's beats.
- **Latency compensation:** if takes land late, raise the slider. Use headphones so the microphone doesn't pick up the other loops.

### Canvas and keys

| Action | Input |
|---|---|
| Zoom | Scroll, pinch, or `+` / `-` |
| Pan | Drag empty space |
| Move a circle | Drag it |
| Zoom to a circle | Double-click it |
| Fit everything | `F` |
| Play / stop | `Space` |
| Record the selected loop, or a new one | `R` |
| Mute the selected loop | `M` |
| Delete the selected loop | `Delete` |
| Deselect | `Esc` |

## How sync works

Each loop has a start time and a length. Every cycle is scheduled at exactly `start + n × length` on the Web Audio clock instead of looping a buffer, so loops with ratios like 5/4 never drift apart. A linked loop shares its parent's start time.

Recording uses an AudioWorklet that copies every microphone frame into a ring buffer indexed by audio-clock frame. A take is cut from that buffer at the exact cycle boundary, shifted by the latency compensation.

## Limits

- Loops are at most 120 seconds long.
- Nothing is saved yet, so loops are gone when you reload the page.
