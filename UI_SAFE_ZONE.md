# TikTok LIVE Portrait UI — Safe Zone Design Specification

**Reference:** user-provided 900×1600 screenshot of TikTok LIVE gaming UI (12 Nov 2026). **This document is a design spec, not an implemented interface.** TikTok controls vary by device, host/viewer mode, OS and LIVE feature configuration; measure actual overlays during a test stream.

## Design goal
Render KINGDOM CHAOS as a **1080×1920 portrait game feed**, while TikTok adds its own LIVE UI on top. Keep critical combat, timers and player identity clear, **do not collide with chat and typing controls at bottom**, and reserve the **right side for gift effects and gift legend**. Don't put important buttons underneath TikTok's native buttons.

## Screenshot analysis
- Top area: host/avatar, join, viewer count, ranking, gallery and system badges occupy top ~0–210px of the 900×1600 screenshot.
- Right side around y≈310–660px: game gift panel in the reference.
- Bottom: chat messages and LIVE action/input controls occupy roughly y≈1000–1600px; this is highly variable.
- Gameplay focus is centered around y≈550–950px in the reference.
- TikTok's overlay is not under our control; the game background and HUD must anticipate occlusion.

## Safe layout on 1080×1920
Use **normalized coordinates** as the source of truth. Approximate reference scaling 1.2×; reserve extra margin for devices with larger controls.

| Region | x | y | Content | Policy |
|---|---:|---:|---|---|
| TOP SYSTEM RESERVED | 0–1080 | 0–300 | Native TikTok header, rankings | No essential game text |
| LEFT GAME STATUS | 36–670 | 315–495 | Round timer, red/blue castle HP, team score | Short text, high contrast; avoid TikTok popups |
| RIGHT GIFT RAIL | 745–1045 | 365–1060 | 4 gifts + effects + current cooldown / queue | **Dedicated right-side panel**; not native interactive controls |
| CENTRAL BATTLE ARENA | 35–735 | 495–1160 | 3D characters, castles, dragon, combat | Main focal region; ensure center readable |
| LOWER GAME INFO | 35–735 | 1160–1290 | War Energy + concise CTA, optionally MVP | Only secondary info; test against actual chat |
| BOTTOM CHAT SAFE SPACE | 0–1080 | **1290–1920** | TikTok chat, keyboard, gift/send/share buttons | **No important game UI**; ambient/background only |

**Conservative design:** for 1080×1920, place all essential UI above y=1290. When chat is expanded or keyboard visible, bottom may cover even more. In test mode overlay the TikTok screenshot as a transparent guide to verify collisions.

### Diagram (not to scale)
```text
1080 x 1920  PORTRAIT
┌──────────────────────────────────────┐ y=0
│    TIKTOK HEADER / RANKING           │
│    KEEP CRITICAL GAME UI OUT         │ y=300
├──────────────────────────┬───────────┤
│ RED/BLUE HP • ROUND TIME │           │
│                          │ RIGHT     │
│       3D BATTLE          │ GIFT      │
│  castle / army / boss    │ RAIL      │
│  animated attack combos  │ 🌹 Rose   │
│                          │ 🍩 Donut  │
│                          │ 🫶 Heart  │
│                          │ 💸 Gun    │
│                          │           │
├──────────────────────────┴───────────┤ y=1160
│ WAR ENERGY • TAP ♥ • JOIN            │
├──────────────────────────────────────┤ y=1290
│                                      │
│      RESERVED FOR TIKTOK CHAT        │
│      USERNAME / COMMENTS             │
│      LIVE INPUT + GIFT BUTTONS       │
│                                      │
│        NO ESSENTIAL GAME UI          │
└──────────────────────────────────────┘ y=1920
```

## Right gift rail — must be legible on mobile
**Placement:** x=745–1045, y=365–1060; width=300px. Matte dark translucent panel with 4 stacked items, 12–16px inner padding, max 2 short lines per item. Never let giant VFX or text cover the rail for more than 2 seconds.

| Gift | Right-side row label | Visual state |
|---|---|---|
| 🌹 Rose (1 coin) | `ROSE • +1 SOLDIER` | icon, summon count / active reserve |
| 🍩 Doughnut (30 coins) | `DOUGHNUT • HEAL` | supply icon, 5s buff countdown |
| 🫶 Hand Heart (100 coins) | `HAND HEART • CHAMPION` | champion portrait, 25s duration |
| 💸 Money Gun (500 coins) | `MONEY GUN • DRAGON` | dragon icon, 3 sequential fireball stages |

**Additional right-rail behavior:** animate the active gift row only (pulse/glow), highlight the gifter username in a floating chip above the rail, queue indicator `NEXT: 2` if needed, show `WAITING` when queued. This is a **game HUD guide**; native TikTok gift tray still opens independently at the bottom.

## Critical UX rules
1. **Do not place game CTA at bottom edge.** Chat and input may cover it. CTA goes under battle, y≈1190–1270, and may auto-hide when occluded.
2. **Don't duplicate native chat.** Viewer messages belong to TikTok overlay; game may display a single small `@user joined` chip near battle.
3. **Safe zone responsive:** all coordinates proportional to 1080×1920, recompute on resize, scale HUD fonts using clamp; test 9:16 and other crop modes.
4. **Gift rail right:** reserve rightmost ~28% of game width; battlefield camera framing must not place essential units behind rail.
5. **Name tags:** show only hero/gift target usernames; don't overlay 500 name tags or cover gift descriptions.
6. **No fake interactive gift buttons:** rail is explanatory only. Actual gifts sent via TikTok's native gift interface.
7. **Dragon cinematic:** start at center, fly over arena, fire 3 times; avoid covering right gift panel and lower chat. Limit focus camera movement.
8. **Contrast:** use dark backplate + bright text; avoid fine-print. Minimum suggested 32–38px for primary gift labels at 1080p, then verify on a physical phone.
9. **Adaptive overlay presets:** `viewer-safe` (bottom reserve 33%), `chat-heavy` (bottom reserve 40%), `studio-preview` (for operator), and toggles for TikTok UI changes.
10. **Operator QA:** overlay a semi-transparent screenshot during preview; test 0, 20, 100 comments, opened gift tray, keyboard, device notch and long usernames. Do not show the guide overlay to viewers.

## Suggested UI implementation
- Single 1080×1920 logical canvas for scene; separate HTML/CSS HUD overlay anchored to normalized zones.
- CSS custom properties `--top-safe:15.6%; --bottom-safe:32.8%; --gift-rail-left:69%; --gift-rail-right:3.2%`.
- Use layout grid: battle/status area on left, gift rail right. Avoid absolute pixels alone: preserve aspect, scale typography and safe margins.
- Render on Windows via OBS Browser Source or TikTok LIVE Studio capture; the native TikTok UI overlays the stream afterward.
- Store zones in config `uiSafeZones` so they can be tuned without touching core game engine.
- The supplied screenshot is **reference for layout only**, not a reusable third-party graphic asset.

## Acceptance criteria
- [ ] Castle HP and timer visible with native TikTok header shown.
- [ ] Right gift rail displays all 4 gifts with no clipped descriptions.
- [ ] Chat messages and text input do not overlap **any essential game UI**.
- [ ] Money Gun animation doesn't hide the gift rail or chat area.
- [ ] Follow, tap and gift feedback remains readable at phone size.
- [ ] Preview comparison on actual phone screen and adjust presets.
