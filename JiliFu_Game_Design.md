# 吉利赋 Browser Game Design

## Summary

`吉利赋` is a first-person, node-based ritual exploration game for the browser. The player opens an old book, enters a private temple, performs small folk-ritual interactions, and gradually restores the temple through `福缘值`.

The MVP should feel like a calm, mysterious nightly visit rather than a management sim. The main fantasy is: white-noise life outside, private sacred space inside.

## Player Experience

- Camera: static first-person illustrated scenes with visible clickable areas for forward navigation, plus a single back button.
- Session length: 3-7 minutes per visit.
- Main verbs: enter, observe, offer incense, toss coin, make a wish, cast moon blocks, collect omen text, restore.
- Tone: quiet, ritualistic, slightly uncanny, with warm reward moments.
- Failure state: no hard fail. Rituals can produce vague or unfavorable signs, encouraging another visit.

## MVP Loop

1. Start at the book page, recite the first charm, and enter the temple.
2. Move through `山门入口`, `前院`, `铜钟池`, and `主殿`.
3. Perform one or more rituals:
   - Toss coin at the bronze bell.
   - Offer incense.
   - Type or speak a wish.
   - Cast moon blocks for an answer.
4. Gain `福缘值`, `吉兆`, or a short `签文`.
5. Spend `福缘值` on one restoration step: cleaner courtyard, brighter lanterns, repaired statue, unlocked side hall.
6. Return on another real-world day for altered lighting, weather, and festival variants.

## Core Systems

- Scene graph: each background scene has exits, hotspots, ambient audio, and optional ritual actions.
- Wish parser: MVP uses text keywords first. Later browser speech input can feed the same parser.
- Omen system: returns one of three ritual tones: blessed, ambiguous, blocked.
- Temple growth: persistent save data stores `fate`, `incense`, `statueLevel`, unlocked scenes, and last visit date.
- Calendar layer: browser date selects day/night and festival overlays. Lunar calendar can be added after the core loop works.
- 诵经声音规则: 全局背景音持续循环播放；进入主殿时，在背景音上叠加播放一次诵经。主殿内诵经为 100% 原声音量；离开主殿时诵经不暂停，继续计时。主殿外近处使用室外听室内效果，音量降低并削弱高频、加入轻微延迟扩散；回到山门时切换成远处模式，音量为 10%，高频进一步削弱，延迟扩散更明显，像很远处的大殿里隐约有一点声响。返回主殿时恢复 100% 原声，听到当前播放进度。诵经完整播放一次后停止，不自动重播；下一次重新进入主殿时再从头触发。此规则后续可以调整。

## Art Direction

- Composition: flat illustrated temple nodes inspired by Chinese landscape painting, scroll paper texture, red lacquer architecture, dark ink silhouettes.
- Palette: parchment, cinnabar, lamp black, weathered gold, muted mineral blue.
- Motion: slow smoke, gold motes on successful rituals, brief red seal stamps, scene dissolves.
- UI material: translucent paper strips, ink borders, cinnabar active states, small seal-like buttons.
- Avoid: dense dashboard panels, generic fantasy RPG chrome, constant animation, health bars, quest clutter.

## Browser Implementation

- Recommended stack: Vite + TypeScript + Phaser for the finished browser game.
- Rendering: Phaser canvas for scenes, particles, hotspots, and transition effects.
- UI: DOM overlay for journal, wish input, settings, and accessible text.
- Assets: background images grouped by scene, FX sprites grouped by ritual, audio grouped by ambience and reward.
- Save: localStorage for MVP, with a small JSON schema that only stores serializable game state.

## MVP Scene List

- `Book`: old market book, incantation entry, save/load access.
- `Gate`: temple name plaque, first arrival, weather variant.
- `Courtyard`: incense burner, navigation hub, restoration level visible.
- `Bell Pond`: drag or flick coin toward bell target.
- `Main Hall`: statue, incense, kneel, wish, moon blocks.
- `Omen Closeup`: generated sign text and reward feedback.

## Test Plan

- Desktop and mobile layouts keep ritual controls reachable without covering the central scene.
- Scene exits are clickable areas inside the illustration, keyboard reachable, and do not require left/right navigation buttons.
- Wish categories correctly detect finance, health, love, study, family, and fallback wishes.
- Moon block results produce all three outcomes over repeated casts.
- Save data persists fate points, statue level, and unlocked scene state after refresh.
- Reduced-motion setting disables non-essential drifting smoke and particle loops.

## Prototype

Open `index.html` in this folder to view the browser design prototype. The current prototype uses original generated SVG scene art in `assets/generated-*.svg`, with the gate scene set to `assets/gate-custom.png`, the main hall exterior set to `assets/main-hall-exterior-custom.png`, and the main hall interior set to `assets/main-hall-custom.png`.
