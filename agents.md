# 🗺️ Pokemon G/S Battle Simulator — File Structure Map

> **File:** `Pokemon.html`  
> **Total Lines:** 6,090  
> **Total Size:** ~250 KB  
> **Architecture:** Single-file monolith (HTML + CSS + JavaScript)

---

## 📐 High-Level Structure

| Section | Lines | Description |
|---------|-------|-------------|
| **HTML Head + CSS** | 1–1,978 | Document metadata, all styles, animations, and keyframes |
| **HTML Body** | 1,979–2,076 | DOM structure — all screens, HUDs, menus, and overlays |
| **JavaScript** | 2,077–6,087 | All game logic — constants, modules, and engine |
| **Initialization** | 6,083–6,087 | Bootstrap code |

---

## 🎨 Section 1: CSS & Styles (Lines 8–1,978)

### 1.1 Design System / CSS Variables (Lines 11–22)
```
:root {
  --bg-color, --screen-bg, --ui-border
  --hp-green, --hp-yellow, --hp-red
  --text-color, --exp-bar
}
```

### 1.2 Core Layout (Lines 23–100)
- `body` — centered flex layout
- `.game-boy` — Game Boy shell container (320×288 viewport)
- `.screen` — inner screen area with pixel font
- `@font-face` — Press Start 2P (retro pixel font)

### 1.3 UI Component Styles (Lines 100–800)

| Component | Lines | Purpose |
|-----------|-------|---------|
| `.scene` | ~100–140 | Battle scene container |
| `.sprite` | ~140–180 | Pokémon sprite positioning & rendering |
| `.hud` / `.hud-active` | ~180–280 | Player/Enemy HUD panels (HP bars, names, levels) |
| `#dialog-box` | ~280–330 | Bottom text/dialog area |
| `#action-menu` | ~330–400 | FIGHT / PKMN / PACK / RUN menu grid |
| `#move-menu` / `.move-btn` | ~400–572 | Move selection grid + info panel |
| `.move-info-grp` | 572–637 | Move type/power display |
| `.confirm-btn` | ~637–670 | Generic confirmation buttons (variants: secondary, nav, danger) |

### 1.4 Screen-Specific Styles (Lines 670–900)

| Screen | Lines | Purpose |
|--------|-------|---------|
| Party Screen | ~670–770 | `.party-slot`, `.slot-icon`, `.mini-hp-*` |
| Pack/Bag Screen | ~770–850 | `.pack-screen`, `.bag-icon`, `.pack-item` |
| Summary Panel | ~850–900 | `.summary-panel`, stat grids, type tags |

### 1.5 Title & Start Screens (Lines 900–1,223)

| Screen | Lines | Purpose |
|--------|-------|---------|
| Start Screen | ~900–1,000 | Title art, `START` button, LCD checkbox |
| Continue Screen | ~1,000–1,060 | Save preview, `CONTINUE` / `NEW GAME` |
| Name Input Screen | ~1,060–1,100 | Player name entry |
| Selection Screen | ~1,100–1,223 | Starter Pokémon picker (3 Pokéballs on table) |

### 1.6 Visual Effects & Animations (Lines 1,223–1,612)

| Animation | Lines | Purpose |
|-----------|-------|---------|
| `.menu-item` styles | 1,223–1,285 | Menu item formatting |
| `spriteFlashPurple/Red/Blue` | ~1,260–1,310 | Status effect sprite flashes |
| `.smoke-particle` | ~1,310–1,350 | Pokéball throw smoke VFX |
| `.shiny-star` / `@sparkle` | ~1,350–1,400 | Shiny encounter sparkle effect |
| `.pokeball-anim` | ~1,400–1,500 | Capture throw + shake + caught animations |
| `@captureThrow` | ~1,450 | Ball arc trajectory |
| `@captureShake` | ~1,470 | Ball wobble on ground |
| `@flashWhite` | ~1,510 | Game Over white flash |
| `.boss-intro` / `.boss-name` | ~1,520–1,560 | Boss encounter visual flair |
| `anim-enter` / `anim-faint` | ~1,560–1,590 | Sprite enter (scale up) and faint (fall down) |
| `shake` / `flicker` | 1,595–1,640 | Hit reaction animations |
| Stat up/down arrows | ~1,640–1,700 | `anim-stat-up`, `anim-stat-down` CSS arrows |

### 1.7 Type-Specific Screen Flashes (Lines 1,700–1,940)
- `.fx-fire` — Red/orange flash
- `.fx-water` — Blue flash
- `.fx-ice` — White/cyan flash
- `.fx-grass` — Green flash
- `.fx-electric` — Yellow flash
- `.fx-psychic` — Purple flash

### 1.8 Status Animation Classes (Lines ~1,850–1,940)
- `.status-anim-brn` — Red silhouette flash (burn)
- `.status-anim-psn` — Purple silhouette flash (poison)
- `.status-anim-frz` — Cyan brightness flash (freeze)
- `.status-anim-par` — Yellow flash (paralysis)
- `.status-anim-slp` — Dim/darken (sleep)
- `anim-violent` — Screen shake (rage)
- `anim-deflect` — Boss ball deflection

### 1.9 Retro Mode & Scrollbar (Lines 1,944–1,978)
- `body.retro-motion *` — Steps-based animation timing
- Custom scrollbar styling

---

## 🏗️ Section 2: HTML Body / DOM Structure (Lines 1,979–2,076)

```
<body>
├── #game-boy                          → Game Boy shell
│   ├── #screen                        → Inner viewport
│   │   ├── #lcd-overlay               → LCD filter overlay
│   │   ├── #start-screen              → Title screen
│   │   │   ├── .title-art             → ASCII logo
│   │   │   ├── #start-btn             → START button
│   │   │   ├── #lcd-check             → LCD toggle checkbox
│   │   │   └── #motion-check          → Retro motion toggle
│   │   ├── #continue-screen           → Save file preview
│   │   │   ├── Save stats display
│   │   │   ├── #save-preview          → Party icon row
│   │   │   ├── #opt-continue          → CONTINUE button
│   │   │   └── #opt-newgame           → NEW GAME button
│   │   ├── #name-screen               → Player name input
│   │   ├── #selection-screen          → Starter selection
│   │   │   ├── #lab-table             → 3 Pokéball slots
│   │   │   ├── #sel-preview-*         → Preview sprite area
│   │   │   └── #sel-text-box          → Selection prompt text
│   │   ├── #scene                     → Battle arena
│   │   │   ├── #enemy-sprite          → Enemy Pokémon sprite
│   │   │   ├── #player-sprite         → Player Pokémon sprite
│   │   │   └── #fx-container          → Visual effects layer
│   │   ├── #enemy-hud                 → Enemy HP/Name/Status HUD
│   │   ├── #player-hud                → Player HP/Name/Status/EXP HUD
│   │   ├── #streak-box                → Win counter display
│   │   ├── #dialog-box                → Bottom text area
│   │   │   ├── #text-content          → Typewriter text output
│   │   │   └── #action-menu           → FIGHT/PKMN/PACK/RUN grid
│   │   ├── #move-menu                 → Move selection (dynamic)
│   │   ├── #pack-screen               → Bag/inventory screen
│   │   ├── #party-screen              → Party management screen
│   │   │   ├── #party-list            → Party slot list
│   │   │   ├── #party-context         → Context menu (SHIFT/SUMMARY)
│   │   │   └── #party-close-btn       → Close/cancel button
│   │   └── #summary-panel             → Pokémon detail view
│   │       ├── Stats, moves, types
│   │       └── .sum-buttons           → Action buttons
│   └── .controls-row                  → Decorative D-pad + buttons
```

---

## ⚙️ Section 3: JavaScript Engine (Lines 2,077–6,087)

### 3.1 Constants & Configuration (Lines ~2,077–2,300)

#### `DEBUG` Object (Lines ~2,077–2,110)
```js
DEBUG = {
  ENABLED: false,
  PLAYER: { ID, LEVEL, SHINY, MOVES, STATUS, STAGES, RAGE, VOLATILES },
  ENEMY:  { ID, LEVEL, SHINY, IS_BOSS, STATUS, STAGES, RAGE, VOLATILES },
  INVENTORY: { ... },
  LOOT: { FORCE_ITEM, MID_BATTLE_RATE }
}
```

#### `ENCOUNTER_CONFIG` (Lines ~2,110–2,125)
- Boss chance, boss streak trigger, level ranges, shiny chance

#### `GAME_BALANCE` (Lines ~2,125–2,160)
- Catch rate modifier, rage triggers, crit chances
- Damage variance, heal percentages
- Rage recoil/multihit parameters

#### `LOOT_SYSTEM` (Lines ~2,160–2,195)
- Drop rates (wild, boss, mid-battle)
- Loot table with weighted scaling

#### `ITEMS` Dictionary (Lines ~2,195–2,240)
- Potions: `potion`, `superpotion`, `hyperpotion`, `maxpotion`
- Revival: `revive`
- Balls: `pokeball`, `greatball`, `ultraball`, `masterball`
- Each has: `name`, `type`, `desc`, `heal/rate`, `img`, `css`

#### `STATUS_DATA` (Lines ~2,240–2,260)
- Status conditions: `brn`, `psn`, `par`, `frz`, `slp`
- Each has: `name`, `color`, `msg`

#### `TYPE_CHART` (Lines ~2,260–2,300)
- Full 18-type effectiveness matrix (Gen II)

#### `STAGE_MULT` (Lines ~2,300–2,310)
- Stat stage multipliers (-6 to +6)

#### `WEATHER_FX` (Lines ~2,310–2,325)
- `sun`, `rain`, `sand`, `hail` — colors, messages, continue text

#### `ANIM` Timing Constants (Lines ~2,325–2,345)
- Intro delays, switch animations, faint delays, catch shakes, etc.

#### `SFX_LIB` (Lines ~2,345–2,365)
- Sound effect name → frequency/type mappings

### 3.2 Utility Functions (Lines ~2,365–2,400)

| Function | Purpose |
|----------|---------|
| `wait(ms)` | Promise-based delay |
| `sleep(ms)` | Alias for `wait` |
| `RNG.roll(chance)` | Random boolean with probability |
| `RNG.int(min, max)` | Random integer in range |
| `StatCalc.recalculate(p)` | Recalculates all stats after level up |

### 3.3 `MOVE_DEX` — Special Move Logic (Lines ~2,400–2,520)

Lookup table for moves with unique behavior. Each entry can have:
- `fixedDamage` — Flat damage (e.g., Sonic Boom = 20)
- `damageCallback(a, d)` — Custom damage formula
- `ohko` — One-hit KO flag
- `isUnique` — Has custom `onHit` handler
- `condition` — Pre-check function

**Notable entries:**
| Move | Type | Description |
|------|------|-------------|
| `SONIC BOOM` | Fixed | Always deals 20 damage |
| `DRAGON RAGE` | Fixed | Always deals 40 damage |
| `SEISMIC TOSS` / `NIGHT SHADE` | Callback | Damage = attacker's level |
| `SUPER FANG` | Callback | Deals 50% current HP |
| `PSYWAVE` | Callback | Random × level damage |
| `FISSURE` / `SHEER COLD` / `GUILLOTINE` / `HORN DRILL` | OHKO | Instant KO if level allows |

### 3.4 `MOVE_LOGIC` — Move Behavior Registry (Lines ~2,520–2,830)

Keyed by move API ID. Categories:

| Type | Example Moves | Behavior |
|------|---------------|----------|
| `charge` | fly, dig, solar-beam, skull-bash | Two-turn moves with optional invuln |
| `recharge` | hyper-beam | Skip next turn after use |
| `protect` | protect, detect | Block incoming attacks |

**Special `MOVE_DEX` entries with `isUnique: true`:**
| Move | Lines | Behavior |
|------|-------|----------|
| `SUBSTITUTE` | ~2,410 | Creates HP decoy doll |
| `TRANSFORM` | ~2,450 | Copies opponent's stats/moves/types |
| `REST` | ~2,485 | Full heal + self-inflict sleep |
| `METRONOME` | ~2,500 | Fetches and executes random move via API |
| `BATON PASS` | ~2,510 | Passes stat stages to switched Pokémon |
| `FOCUS ENERGY` | ~2,515 | Boosts crit rate |
| `ROAR` / `WHIRLWIND` | ~2,520 | Force switch (calls `_forceSwitchOrRun`) |
| `DREAM EATER` | ~2,530 | Only works on sleeping targets |
| `FUTURE SIGHT` | ~2,540 | Delayed attack (3 turns) |
| `RAPID SPIN` | ~2,545 | Clears delayed moves targeting user |
| `BELLY DRUM` | ~2,550 | Trades 50% HP for max ATK |
| `CURSE` | ~2,560 | Ghost: curse effect / Other: buff ATK+DEF, nerf SPE |
| `PAIN SPLIT` | ~2,570 | Averages HP between both combatants |
| `PERISH SONG` | ~2,580 | Sets 3-turn faint counter |
| `LEECH SEED` | ~2,590 | Drains HP each turn |
| `DESTINY BOND` | ~2,600 | If user faints, opponent faints too |
| `COUNTER` / `MIRROR COAT` | ~2,610 | Reflects physical/special damage |
| `SWAGGER` | ~2,620 | Raises ATK + confuses |
| `HEAL BELL` | ~2,630 | Cures party status |
| `SYNTHESIS` / `MOONLIGHT` / `MORNING SUN` | ~2,640 | Weather-dependent heal |
| `PRESENT` | ~2,650 | Random: damage or heal opponent |
| `MAGNITUDE` | ~2,660 | Random power (10–150) |

### 3.5 `AudioEngine` Module (Lines 2,840–2,967)

| Method | Purpose |
|--------|---------|
| `init()` | Creates `AudioContext`, initializes oscillators |
| `playCry(url)` | Plays Pokémon cry from URL via `<audio>` element |
| `playTone(freq, type, dur, vol, delay)` | Generates synth tone |
| `playNoise(dur)` | White noise generator (for impact SFX) |
| `playSfx(key)` | Plays named sound effect from `SFX_LIB` |

**Built-in SFX keys:** `select`, `confirm`, `damage`, `crit`, `super_effective`, `not_very_effective`, `miss`, `heal`, `levelup`, `exp`, `swoosh`, `ball`, `throw`, `catch_success`, `catch_click`, `run`, `clank`, `shiny`, `electric`, `fire`, `water`, `ice`, `grass`, `psychic`, `error`, `funfair`, `rumble`

### 3.6 `API` Module (Lines 2,968–3,075)

| Method | Purpose |
|--------|---------|
| `getPokemon(id, level, overrides)` | Fetches from PokéAPI, builds full Pokémon object |
| `getMove(idOrName)` | Fetches single move data (used by Metronome) |

**Pokémon Object Shape:**
```js
{
  id, name, level, types, stats, stages, baseStats,
  currentHp, maxHp, exp, nextLvlExp, baseExp,
  moves: [{ id, name, type, category, power, accuracy, priority, meta, stat_changes, target }],
  frontSprite, backSprite, icon, cry,
  isShiny, isBoss, isHighTier,
  rageLevel, failedCatches,
  volatiles: {}, transformBackup: null
}
```

### 3.7 `Input` Module (Lines 3,077–3,267)

**State:**
- `focus` — Current highlighted UI element index
- `mode` — Current input context
- `lcdEnabled` — LCD overlay toggle

**Modes:** `START`, `CONTINUE`, `NAME`, `BATTLE`, `CONFIRM_RUN`, `MOVES`, `SELECTION`, `BAG`, `PARTY`, `CONTEXT`, `SUMMARY`, `NONE`

| Method | Lines | Purpose |
|--------|-------|---------|
| `init()` | 3,081 | Attaches global keydown listener |
| `setMode(m, resetIndex)` | 3,083–3,088 | Changes input context |
| `visuals{}` | 3,090–3,152 | Mode → highlighted DOM element resolver |
| `updateVisuals()` | 3,155–3,171 | Applies `.focused` class to current element |
| `handlers{}` | 3,173–3,239 | Mode → arrow key / confirm handlers |
| `handleKey(e)` | 3,242–3,266 | Global key dispatcher (back button, nav sounds) |

### 3.8 `StorageSystem` Module (Lines 3,270–3,301)

| Method | Purpose |
|--------|---------|
| `save(data)` | Serializes to `localStorage` |
| `load()` | Deserializes from `localStorage` |
| `exists()` | Check if save exists |
| `wipe()` | Delete save data |

**Storage Key:** `gs_battler_save`

### 3.9 `EncounterManager` Module (Lines 3,303–3,382)

| Method | Purpose |
|--------|---------|
| `determineSpecs(party, wins)` | Calculates enemy level, boss status, shiny chance |
| `generate(party, wins)` | Fetches enemy Pokémon via API with computed specs |

**Scaling Logic:**
- Reference level = average of (party avg level + party max level) / 2
- Boss triggered every N wins or by random chance
- Boss gets 1.5× HP, "BOSS" prefix, rage level 3

### 3.10 `Game` Module (Lines 3,385–4,018)

The main game state manager. Orchestrates flow between screens.

**State Properties:**
```js
party: [], activeSlot: 0, enemyMon: null,
inventory: { potion, superpotion, hyperpotion, maxpotion, revive, pokeball, greatball, ultraball, masterball },
state: 'START', wins: 0, bossesDefeated: 0,
playerName: 'PLAYER', forcedSwitch: false
```

#### Flow Control Methods

| Method | Lines | Purpose |
|--------|-------|---------|
| `toggleLcd()` | 3,391–3,396 | Toggle LCD overlay effect |
| `toggleRetroMotion()` | 3,398–3,403 | Toggle step-based animations |
| `checkSave()` | 3,405–3,428 | Check for existing save, show continue screen |
| `save()` | 3,430–3,439 | Save current game state |
| `load()` | 3,441–3,472 | Load saved game state |
| `startNameInput()` | 3,475–3,480 | Show name entry screen |
| `confirmName()` | 3,482–3,487 | Accept player name |
| `loadGame()` | 3,489–3,541 | Full game resume flow |
| `newGame()` | 3,543–3,549 | Reset and start fresh |
| `resetToTitle()` | 3,551–3,560 | Hard reset to title screen |
| `returnToTitle()` | 3,562–3,568 | Soft return to title |
| `skipBattle()` | 3,570–3,574 | Skip current encounter (used by Roar) |
| `startNewBattle(isFirst)` | 3,576–3,623 | Generate enemy + launch battle |

#### Menu/Screen Methods

| Method | Lines | Purpose |
|--------|-------|---------|
| `showSelectionScreen()` | 3,628–3,672 | Fetch 3 random starters from API |
| `openSummary(p, mode)` | 3,674–3,713 | Open Pokémon detail panel |
| `renderSummaryData(p)` | 3,715–3,743 | Populate summary with stats/moves |
| `navSummary(dir)` | 3,745–3,755 | Navigate between Pokémon in summary |
| `closeSummary()` | 3,757–3,764 | Close summary panel |
| `confirmSelection()` | 3,766–3,775 | Confirm starter pick |
| `openParty(forced)` | 3,777–3,804 | Open party management screen |
| `renderParty()` | 3,806–3,819 | Render party slot list |
| `openContext(index)` | 3,821–3,829 | Open context menu (SHIFT/SUMMARY/CLOSE) |
| `closeContext()` | 3,831 | Close context menu |
| `releasePokemon(index)` | 3,833–3,844 | Release Pokémon from party |
| `applyItemToPokemon(index)` | 3,846–3,858 | Use healing item on specific Pokémon |
| `partySwitch()` | 3,860–3,877 | Switch active Pokémon |

#### EXP & Win Methods

| Method | Lines | Purpose |
|--------|-------|---------|
| `distributeExp(amount, indices, isBoss)` | 3,880–3,895 | Distribute EXP to participants |
| `gainExpAnim(amount, p)` | 3,897–3,906 | Animated EXP bar fill |
| `processLevelUp(p)` | 3,908–3,922 | Level up with stat recalculation |
| `finishWin()` | 3,924–3,960 | Post-win sequence (heal, loot, rage reset, transform revert) |
| `getLoot(enemy, levelDiff)` | 3,962–3,975 | Weighted loot roll |
| `tryMidBattleDrop(enemy)` | 3,977–3,983 | Mid-battle item drop chance |
| `handleWin(wasCaught)` | 3,985–4,003 | Win handler (EXP calc, finalize) |
| `handleLoss()` | 4,005–4,017 | Loss handler (force switch or game over) |

### 3.11 `Battle` Module (Lines 4,022–6,008)

The core battle engine. Handles all combat mechanics, animations, and turn flow.

**State Properties:**
```js
p: null,              // Player Pokémon reference
e: null,              // Enemy Pokémon reference
uiLocked: true,       // Input lock flag
participants: Set(),  // Indices of Pokémon that participated (for EXP)
userInputPromise: null,// Promise resolver for forced switches
weather: { type, turns },
delayedMoves: [],     // Future Sight, etc.
lastMenuIndex: 0      // Cursor memory
```

#### Helper Methods

| Method | Lines | Purpose |
|--------|-------|---------|
| `forceReflow(el)` | 4,027–4,028 | Force DOM reflow for animation restart |
| `triggerHitAnim(target, type)` | 4,035–4,072 | Standardized hit visual (shake + flash + SFX) |
| `applyDamage(target, amount, type)` | 4,075–4,102 | Damage application (substitute-aware) |
| `applyHeal(target, amount, msg)` | 4,106–4,125 | Healing with optional custom message |

#### Visual Methods

| Method | Lines | Purpose |
|--------|-------|---------|
| `spawnSmoke(x, y)` | 4,128–4,141 | Particle burst effect (12 particles) |
| `performVisualSwap(mon, src, isDoll)` | 4,145–4,177 | "Poof" swap animation (substitute/switch) |
| `triggerRageAnim(cry)` | 4,179–4,185 | Screen shake + cry for rage |
| `resetSprite(el)` | 4,187–4,191 | Clear all sprite animation classes |
| `playSparkle(side)` | 5,843–5,847 | Shiny encounter star particles |

#### HUD & Text

| Method | Lines | Purpose |
|--------|-------|---------|
| `updateHUD(mon, side)` | 4,194–4,238 | Full HUD refresh (name, level, HP, status, rage, EXP) |
| `typeText(text, cb, fast)` | 4,241–4,263 | Typewriter text engine (returns Promise) |
| `revertTransform(mon)` | 4,265–4,278 | Undo Transform data changes |

#### Scene Management

| Method | Lines | Purpose |
|--------|-------|---------|
| `resetScene()` | 4,284–4,322 | Nuclear reset (clears all state, animations, weather) |
| `cleanup()` | 4,325–4,327 | Legacy wrapper for `resetScene` |
| `setWeather(type)` | 4,329–4,339 | Set weather + visual + text |

#### Setup & Encounter Flow

| Method | Lines | Purpose |
|--------|-------|---------|
| `setup(player, enemy, playIntro, skip)` | 4,342–4,387 | Initialize battle (sprites, HUDs, menu listeners) |
| `startEncounterSequence(...)` | 4,389–4,458 | Cinematic intro (silhouette → reveal → cry → HUD) |
| `triggerPlayerEntry(mon)` | 4,460–4,482 | Player Pokémon entrance animation |

#### Turn Execution

| Method | Lines | Purpose |
|--------|-------|---------|
| `endTurnItem()` | 4,484–4,504 | Enemy attacks after player uses item |
| `checkCanMove(mon, isPlayer)` | 4,512–4,577 | Status check: flinch, freeze, sleep, para, confusion |
| `processEndTurnEffects(mon, isPlayer)` | 4,580–4,622 | End-of-turn: weather damage, burn/poison |
| `applyStatChanges(target, changes)` | 4,625–4,669 | Stat stage modifications with animations |
| `applyStatus(target, ailment)` | 4,672–4,731 | Status infliction with type animations |
| `processDelayedMoves()` | 4,733–4,755 | Execute Future Sight / scheduled attacks |
| `performTurn(playerMove)` | 4,757–4,817 | Full turn: build queue, sort by priority/speed, execute |
| `performSwitch(newMon)` | 4,819–4,852 | Voluntary switch (triggers enemy attack) |
| `performItem(itemKey)` | 4,854–4,890 | Use item (triggers enemy attack) |
| `performRun()` | 4,892–4,912 | Flee battle (save + return to title) |

#### Queue System

| Method | Lines | Purpose |
|--------|-------|---------|
| `runQueue(queue)` | 4,915–4,955 | Execute action queue + end-of-turn sequence |
| `executeAction(action)` | 4,957–4,963 | Action router (ATTACK/SWITCH/ITEM) |
| `processSwitch(newMon, isFaint)` | 4,965–5,049 | Full switch sequence (Baton Pass aware) |
| `processItem(itemKey)` | 5,051–5,107 | Item logic (balls, potions, revives, buffs) |

#### Type System

| Method | Lines | Purpose |
|--------|-------|---------|
| `getTypeEffectiveness(type, types)` | 5,110–5,119 | Calculate type multiplier |

#### Catch System

| Method | Lines | Purpose |
|--------|-------|---------|
| `attemptCatch(ballKey)` | 5,121–5,238 | Full capture sequence (deflect, throw, shake, catch/fail, rage) |

#### Attack Pipeline

| Method | Lines | Purpose |
|--------|-------|---------|
| `processAttack(attacker, defender, move)` | 5,240–5,409 | Master attack handler (recharge → canMove → protect → charge → accuracy → execute) |
| `executeDamagePhase(attacker, defender, move)` | 5,412–5,472 | Director: routes to damage or status logic |
| `handleDamageSequence(attacker, defender, move)` | 5,475–5,544 | Combo handler: multi-hit, drops, rage |
| `resolveSingleHit(attacker, defender, move, dmg)` | 5,547–5,586 | Single hit: visuals, HP, drain/recoil |
| `handleAttackerRage(attacker, defender, move)` | 5,591–5,628 | Rage extra attacks with reduced damage |
| `handleStatusMove(attacker, defender, move)` | 5,632–5,665 | Status move routing (weather, unique, standard) |
| `handleMoveSideEffects(attacker, defender, move)` | 5,667–5,729 | Post-move: stat changes, status, flinch, explosion |
| `handleFaint(mon, isPlayer)` | 5,731–5,766 | Faint sequence (cry, animation, HUD, delegate) |
| `calcDamage(attacker, defender, move)` | 5,769–5,838 | Gen II damage formula (STAB, types, crits, variance) |

#### UI Methods

| Method | Lines | Purpose |
|--------|-------|---------|
| `uiToMoves()` | 5,849–5,868 | Show move selection menu |
| `uiToMenu()` | 5,871–5,896 | Return to main action menu (handles recharge/charge) |
| `askRun()` | 5,898–5,907 | Show run confirmation (blocked for bosses) |
| `openPack()` | 5,909–5,917 | Open bag/inventory screen |
| `renderPackList()` | 5,918–5,955 | Render inventory items + cancel button |
| `buildMoveMenu()` | 5,957–5,993 | Dynamically generate move button grid |
| `switchIn(newMon, wasForced)` | 5,994–6,004 | Entry point for switching (forced vs voluntary) |

### 3.12 Standalone Functions (Lines 6,010–6,060)

| Function | Lines | Purpose |
|----------|-------|---------|
| `_forceSwitchOrRun(battle, user, target)` | 6,011–6,060 | Helper for Roar/Whirlwind — player: skip battle; enemy: random force switch |

### 3.13 Dynamic CSS Injection (Lines 6,062–6,080)
- Injects `shakeFlipped` keyframes and `.anim-hit-flipped` class at runtime
- Handles substitute doll directional shake fix

### 3.14 Initialization (Lines 6,083–6,087)
```js
Input.init();
Input.setMode('START');
```

---

## 🔄 Game State Machine

```
START → (checkSave) → CONTINUE / NAME
                ↓              ↓
           loadGame()     newGame()
                ↓              ↓
           BATTLE ←←← SELECTION (pick starter)
              ↓
        ┌─────┴─────┐
        ↓            ↓
   WIN/CAUGHT    LOSS/FAINT
        ↓            ↓
   distributeExp  handleLoss
   finishWin      ↓
        ↓      openParty(forced) → switch
        ↓              or
   startNewBattle  newGame() (all fainted)
```

---

## 🔗 External Dependencies

| Dependency | Usage |
|------------|-------|
| [PokéAPI v2](https://pokeapi.co/api/v2) | Pokémon data, moves, sprites, cries |
| [PokeAPI Sprites](https://raw.githubusercontent.com/PokeAPI/sprites/) | Front/back sprites, icons, item images |
| [Google Fonts](https://fonts.googleapis.com) | Press Start 2P pixel font |
| `localStorage` | Save/load system |
| `AudioContext` (Web Audio API) | Synthesized sound effects |

---

## 📝 Notes for Contributors

1. **This is a single-file application.** All HTML, CSS, and JS live in `Pokemon.html`.
2. **No build tools required.** Open directly in a browser or serve via `python3 -m http.server`.
3. **API-dependent.** Requires internet for PokéAPI calls (Pokémon data, sprites, cries).
4. **Module pattern.** JavaScript uses plain object literals (`const Module = { ... }`) — no classes, no imports.
5. **Async/await throughout.** The battle engine is fully asynchronous with Promise-based animations.
6. **Gen II faithful.** Damage formula, type chart, and mechanics target Pokémon Gold/Silver accuracy.
