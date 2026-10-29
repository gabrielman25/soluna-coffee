# Soluna Coffee

Customer site for Soluna Coffee (Katy, TX): pick a store, see the live menu before you come in, and earn rewards points with the code on the in-store menu board.

Live at https://gabrielman25.github.io/soluna-coffee/

## How it connects

The menu board in the shop (`soluna-menu-board.html`, kept off this repo) hosts a PeerJS room. The Elyson board's room name is `Elyson`, which uses the peer id `soluna-coffee-elyson`.

The site only connects when it needs to:

- once when a customer opens a store (skipped if it checked in the last 2 minutes)
- when they tap refresh (2-minute cooldown, 30 seconds after a failed try)
- when they check a rewards code (the answer also carries the latest menu)

Each request opens a connection, gets one answer and closes. The visitor's PeerJS session is dropped after 20 seconds idle.

## Rewards

The board shows a 4-character code that changes every 3 minutes (the previous code still works for 45 seconds). A valid code is worth 5 points, once per day per device; the day resets at 4 AM local time. Points, unlocked items, history and preferences (vibration, animations, open where I left off) are saved in the browser's localStorage.

## Photos and icons

Photos from @solunacoffeeshop go in `assets/photos/` (file names in that folder's README). Icons are Phosphor Icons 2.1.1 (MIT), Bold and Fill weights, inlined in `index.html`.

## Adding a store

Add an entry to `STORES` in `index.html` and set that store's board `ROOM_NAME` to the same room name.

## Game

Set `GAME.url` in `index.html` when the Soluna game is published and it will load inside the arch frame on the Game tab.

## NFC tags

Write one **URI record** to each tag (NTAG213/215/216 all work):

```
https://gabrielman25.github.io/soluna-coffee/?tap=elyson-1NQW26
```

- Tapping any NFC phone (iPhone XS or newer, Android with NFC on) opens the site on the Elyson Rewards tab and adds today's 5 points.
- With the site already open in Chrome on Android, "Turn on tap to collect" lets the page read the tag in place.
- Tag taps and board codes share the same once-a-day check-in (resets at 4 AM).
- The `elyson-1NQW26` part is checked by the site; a new store gets its own value from `tapCheck(storeId)`.
