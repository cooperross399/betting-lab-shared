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

Pages are designed in Claude Design and arrive on their own. Claude Design
pushes a `design/**` branch to `nhl-betting-lab`, whose Design Drop workflow
copies any changed shared file here, commits it to `main` and tags it (`v2`,
`v3`, ...; tags are never moved), re-pins the NHL lock, merges the drop on
green, and then opens a `shared-sync/<tag>` pull request in the EPL lab, the
CBB lab and the hub that copies the tag and re-pins each lock. Those merge
themselves when every check passes.

By hand, if ever needed: commit and tag here, then in each lab run
`python web/sync_shared.py --lock <checkout of this repo> --tag vN` followed by
`--from <checkout>` and open a pull request with the copies and the new lock;
in the hub, copy `lib/sports.js` and `lib/format.js` and update
`shared.lock.json` the same way. Do not edit `design/**` or `shared-sync/**`
branches by hand.

Nothing here fetches odds, spends a credit or places a bet. The logos are the
leagues' and clubs' trademarks, shown for identification only.
