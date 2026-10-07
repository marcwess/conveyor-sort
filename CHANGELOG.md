# Changelog

All notable changes to Conveyor Sort will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.0] - 2026-10-07

### Changed
- **Drag to sort.** Press an item, drag it over its bin, and let go. Tapping an item
  or a bin no longer sorts anything. Built on pointer events, so mouse and touch both
  work. Only one pointer is tracked at a time and extra fingers are ignored. The play
  area uses `touch-action: none`.
  - A held item lifts off the belt (it grows, gets a bigger shadow and sits slightly
    above the finger on touch), and the bin under it lights up. A dashed marker on the
    belt shows where the item would be now; it turns red in the danger zone.
  - Correct bin: a normal sort with the same score, combo and feedback as before.
  - Wrong bin: the bin rejects the item and it counts as a missed item (−1 heart).
  - Released anywhere else, or pointer cancel: the item goes back on the belt where it
    would be by now and keeps moving. If that's past the end, it counts as a miss.
  - Grabbing is forgiving (item radius + 24 px). A bomb only goes off if you press
    squarely on it; otherwise the nearest safe item is picked.
  - Special items: **bombs** go off when touched (−1 heart, same as the old tap).
    **Mystery boxes** open when grabbed and you keep dragging what was inside.
    **Rainbow stars** sort everything when dropped into any bin.
  - Pausing, failing, completing a level or going Home in the middle of a drag puts
    the item back on the belt. You can't grab during the "Ready?" grace period.
- All instructional text now says drag instead of tap (home tagline, level hints,
  on-belt labels). The level 3 tip, which taught tap-a-bin, now explains dropping
  over a bin and the wrong-bin penalty.
- Fail-screen offer: the header is now just "Continue?". Each button states what it
  gives: "+3 hearts · [coin] price" and "+1 heart · Watch ad".
- **Continue price scales with level:** 50 + 10 × level (60 on level 1, up to 150 on
  level 10). Overtime keeps adding +10 per shift, capped at 250. Measured earnings
  are about 49–60 coins for a clean level, so a continue costs roughly 1–2 levels of
  play. It used to be a flat 60. If you can't afford it, the price shows "need more"
  and the button opens the shop.

### Tests
- `test.js` now sends real pointer drags: CDP touch events plus one mouse drag. It
  covers correct bin, wrong bin, drop on nothing, release past the end of the belt,
  tap-doesn't-sort, extra pointer ignored, pause mid-drag, slow-mo, bombs, boxes and
  stars, plus the continue header, button text and price. The auto-play bot drags.
- Added a human-paced check (350 ms reaction + 550 ms drag per item) for levels 1–3.
- New screenshot: `shot-drag.png` (mid-drag over the highlighted bin).

## [1.0.1] - 2026-10-07

### Changed
- "Out of Hearts!" fail screen: the **Continue** offer (60 coins for +3 hearts, or
  watch a demo ad for +1 heart) can now be used **once per level attempt**. After a
  continue has been used, a second fail in the same attempt shows only **Retry** and
  **Home**. Retry, Replay, starting a level, or going Home resets the allowance.
  Visuals are unchanged; the offer box is simply hidden once it's used up.
- `npm test` now runs `node test.js`, and the test script exits non-zero on any failure.

### Fixed
- The Continue / Watch ad buttons can no longer be triggered more than once per fail
  (for example by a double tap, or while the ad is already playing).

### Tests
- Added fail-screen checks: second fail hides the continue offer and keeps Retry/Home;
  hidden buttons can't be triggered; Retry and Home both reset the allowance; a coin
  continue also uses up the allowance.
- New screenshot: `shot-fail-nocontinue.png` (second fail, Retry/Home only).

## [1.0.0] - 2026-10-07

### Added
- First version of the single-file game (`conveyor-sort.html`): 10 levels plus endless
  Overtime, bombs, rainbow stars, mystery boxes, Slow-Mo booster, demo shop/starter pack,
  demo rewarded ad, and saved progress.
