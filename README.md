# Arm Wrestling Duel Overlay

A keyboard and trigger friendly 2-player arm-wrestling mini-game built for livestream overlays. Each viewer interaction fires a single input for either competitor, pushing the shared strength meter toward their side. Rounds loop endlessly and require zero manual resets.

## Running the mini-game

1. Open `index.html` in any modern browser (Chrome, Edge, Firefox). No build steps are needed.
2. Add the page as a browser source in OBS/Streamlabs to turn it into an overlay.
3. Resize freely—the layout is responsive and keeps every HUD element readable on small screens.

## Core mechanics

- Player A (left side) presses **A**. Player B (right side) presses **L**.
- Each valid input adds a fixed +5 strength toward that player: +5 pushes the meter toward Player A, −5 toward Player B.
- Strength ranges from **−100 to +100**. Neutral lock is 0.
- Hitting +100 (Player A) or −100 (Player B) instantly declares a win, bumps that player’s win counter, shows a win banner, and automatically resets after ~1.2s.
- There is no decay, timing bonus, or randomness—every trigger is deterministic and identical.

## Mapping external triggers

The script exposes a helper so bots, channel point rewards, MIDI pads, or other automations can simulate inputs without emulating keyboard presses:

```js
window.armWrestleTrigger('A'); // Counts as Player A pressing the key once
window.armWrestleTrigger('B'); // Counts as Player B pressing the key once
```

Examples:

- **Twitch chat bot**: When a user redeems “Left Punch,” call `armWrestleTrigger('A')` via a browser-source bridge or websocket.
- **Stream Deck / Touch Portal**: Configure a hotkey action that runs `javascript:armWrestleTrigger('B')` in the embedded browser source.
- **Custom server**: If you receive webhook hits, forward them through OBS’ browser source using `window.postMessage` and call the helper upon receipt.

As long as the trigger calls the helper with `'A'` or `'B'`, it behaves the same as a viewer pressing the keyboard key once.

## Files

- `index.html` – Self-contained HTML/CSS/JS mini-game with HUD, animation loop, deterministic progress logic, win detection, automatic round resets, and exposed trigger API.

Enjoy the hype!
