# betting-lab-shared

The page files the NHL, EPL and CBB labs publish byte-identical, and each lab's
team logos (128 px). Each lab pins one tag of this repository in
`web/shared.lock.json`, with the SHA-256 of every file it takes, and its publish
run copies the tag in with `web/sync_shared.py --from`. A file that differs from
the lock fails that run, so nothing drifts silently.

- `web/Board.dc.html`, `Results.dc.html`, `Archive.dc.html`, `Graphic.dc.html`
- `web/lib/live.js`, `lib/format.js`, `lib/sports.js` (the hub serves these two
  from the same tag)
- `web/logos/<sport>/` (`nhl`, `epl`, `cbb`), with each folder's `SOURCES.md`

## Changing a file

NHL is the editing home for page design drops. A drop lands here first:

1. Commit the change here and tag it (`v2`, `v3`, ...). Tags are never moved.
2. In each lab, copy the changed files into `web/`, run
   `python web/sync_shared.py --lock <checkout of this repo> --tag vN`, and open a
   pull request with the copies and the new lock. Its tests compare the copies
   to the lock.
3. In the hub, copy `lib/sports.js` and `lib/format.js` and update
   `shared.lock.json` the same way.

Nothing here fetches odds, spends a credit or places a bet. The logos are the
leagues' and clubs' trademarks, shown for identification only.
