# Chain Reaction

A two-phone take on the classic Chain Reaction board game. One HTML file, no build step, no accounts.

- **Play online.** Tap "Play online", send the link, and the game starts when the other person joins. Hearts, nudges and a running score between the two of you.
- **Pass and play.** Two to eight players sharing one device, with undo.
- **Boards.** 6×9 (the classic), 8×12 and 10×15.
- **Pieces.** Play as the classic orb in your colour, or as an emoji: pick from the brainrot set (💀 🗿 😭 🥀 🤡 🦈 🐊 and friends) or paste any emoji. Tap your name during a game to swap.

## Rules

1. Take turns dropping an orb into an empty cell or one of your own.
2. A cell bursts when it holds as many orbs as it has neighbours: 2 in a corner, 3 on an edge, 4 in the middle.
3. A burst throws one orb into each neighbour and takes those cells over, orbs and all.
4. Bursts set off more bursts. Wipe out your opponent's last orb to win.

## How online play works

There is no game server. A room's state is its list of moves, published as a retained MQTT message on two public brokers (EMQX and HiveMQ) over secure WebSockets. Each phone replays the moves through the same rules engine, so both boards always match. A phone that drops off (screen locked, app switched) catches up from the retained state when it comes back.

Anyone who knows a room code can read that room's traffic, so the game only asks for a first name.

## Dots and Boxes

`dots/` holds a second game built the same way: [pawanpere.github.io/chain-reaction/dots/](https://pawanpere.github.io/chain-reaction/dots/).

- **Play online with two to four people.** Pick how many seats when you make the room. The game starts once every seat is filled, or the host can start early with whoever has joined.
- **Pass and play** for two to four, with undo.
- **Boards** of 3×3, 5×5 and 6×8 boxes.
- **Stamps.** Captured boxes show your initial in your colour, or any emoji you pick.

Rules: take turns drawing a line between two neighbouring dots. Draw the fourth side of a box and it's yours, and you go again. When every line is drawn, most boxes wins. It uses the same relay as Chain Reaction, on its own topic.

## Run it locally

```bash
python3 -m http.server 8000
```

Open http://localhost:8000 (or http://localhost:8000/dots/). To play both sides on one computer, open a second tab with `?p=b` added before the `#` in the invite link. That tab keeps its own identity.
