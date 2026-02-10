<p align="center">
  <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/250.png" width="120" alt="Ho-Oh">
  <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/249.png" width="120" alt="Lugia">
</p>

<h1 align="center">⚔️ Pokémon G/S Battle Simulator</h1>

<p align="center">
  <strong>A faithful Pokémon Gold/Silver battle system clone running entirely in the browser.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Generation-II-gold?style=for-the-badge" alt="Gen II">
  <img src="https://img.shields.io/badge/Pokémon-251-red?style=for-the-badge" alt="251 Pokemon">
  <img src="https://img.shields.io/badge/Engine-Vanilla_JS-yellow?style=for-the-badge" alt="Vanilla JS">
  <img src="https://img.shields.io/badge/Lines-6090-blue?style=for-the-badge" alt="6090 Lines">
  <img src="https://img.shields.io/badge/Dependencies-Zero-green?style=for-the-badge" alt="Zero Dependencies">
</p>

---

## 🎮 What Is This?

A fully-featured Pokémon battle simulator inspired by **Pokémon Gold & Silver**, built as a **single HTML file** with zero dependencies. It renders inside a pixel-perfect Game Boy Color shell and features:

- 🏟️ Full turn-based battle engine with Gen II damage formula
- 📡 Live Pokémon data from [PokéAPI](https://pokeapi.co/) (all 251 Gen I & II Pokémon)
- 🎵 Synthesized 8-bit sound effects via Web Audio API
- 💾 localStorage save/load system
- 🎨 Retro pixel art aesthetic with LCD filter option

---

## ✨ Features

### ⚔️ Battle System
- **Gen II Damage Formula** — STAB, type effectiveness (18-type chart), critical hits, stat stages (-6 to +6), random variance
- **Priority System** — Speed ties, move priority, action queue sorting
- **Weather** — Sun, Rain, Sandstorm, Hail with damage modifiers and end-of-turn effects
- **Status Conditions** — Burn, Poison, Paralysis, Freeze, Sleep with accurate mechanics
- **Volatile Statuses** — Confusion, Flinch, Substitute, Invulnerability (Fly/Dig), Recharge, Focus Energy

### 🧠 Move System (80+ Unique Behaviors)
- **Two-Turn Moves** — Fly, Dig, Solar Beam, Skull Bash (with weather skip)
- **Recharge Moves** — Hyper Beam
- **Protection** — Protect, Detect (fails on consecutive use)
- **OHKO Moves** — Fissure, Sheer Cold, Guillotine, Horn Drill
- **Fixed Damage** — Sonic Boom (20), Dragon Rage (40), Seismic Toss (level-based)
- **Field Effects** — Future Sight, Leech Seed, Perish Song, Destiny Bond
- **Unique Moves** — Transform, Metronome, Baton Pass, Substitute, Rest, Pain Split, Belly Drum, Swagger, Counter, Mirror Coat, Rapid Spin, Heal Bell, Present, Magnitude, Curse, and more

### 🎒 Item System
| Item | Effect |
|------|--------|
| Potion / Super / Hyper / Max | Heal 20 / 50 / 200 / Full HP |
| Revive | Revive fainted Pokémon at 50% HP |
| Poké Ball / Great / Ultra / Master | Catch wild Pokémon (scaling catch rates) |

### 😤 Rage System (Original Mechanic)
- Pokémon build **rage** when taking damage or missing attacks
- Rage level (1–3) grants bonus attacks with diminishing power
- Enraged Pokémon can **deflect Poké Balls** (bosses)
- Rage includes recoil risk as a balancing mechanic

### 👹 Boss Encounters
- Triggered periodically based on win streak
- **1.5× HP** multiplier with "BOSS" prefix
- Start at **max rage** (level 3)
- **Cinematic intro** with screen shake and silhouette reveal
- Team-wide EXP distribution on defeat

### 🎰 Loot System
- Items drop from defeated/caught Pokémon
- Weighted loot table that scales with enemy strength and win streak
- Mid-battle drops possible during combat
- Boss encounters have higher drop rates

### 🐣 Catch System
- Accurate Gen II catch formula
- Poké Ball shake animations (1–3 shakes)
- Party management with overflow release mechanic (max 6)
- Caught Pokémon receive partial HP restoration

### 💾 Save System
- Auto-saves after each battle
- Saves: party, inventory, win streak, active battle state
- Resume mid-battle with weather and delayed moves preserved
- Save preview on continue screen with party icons

---

## 🚀 Getting Started

### Option 1: Direct Open
Simply open `Pokemon.html` in any modern browser:
```bash
# macOS
open Pokemon.html

# Linux
xdg-open Pokemon.html

# Windows
start Pokemon.html
```

### Option 2: Local Server (Recommended)
```bash
# Python 3
python3 -m http.server 8000

# Then navigate to http://localhost:8000/Pokemon.html
```

### Option 3: VS Code Live Server
1. Install the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension
2. Right-click `Pokemon.html` → "Open with Live Server"

> **Note:** Internet connection required for PokéAPI data (sprites, moves, cries).

---

## 🕹️ Controls

### Keyboard
| Key | Action |
|-----|--------|
| `↑ ↓ ← →` | Navigate menus |
| `Z` / `Enter` | Confirm / Select |
| `X` | Back / Cancel |

### Mouse
- Click any button or menu item directly
- Hover to preview Pokémon and move info

---

## 🏗️ Architecture

This project is a **single-file monolith** — all HTML, CSS, and JavaScript live in one `Pokemon.html` file (6,090 lines, ~250KB).

### Module Overview

```
Pokemon.html
├── CSS (~1,978 lines)
│   ├── Design System (CSS variables, fonts)
│   ├── Layout (Game Boy shell, screens)
│   ├── Components (HUD, menus, buttons)
│   ├── Animations (50+ keyframes)
│   └── Type-specific VFX
│
├── HTML (~100 lines)
│   └── DOM Structure (screens, HUDs, overlays)
│
└── JavaScript (~4,000 lines)
    ├── Constants & Config
    │   ├── DEBUG          — Developer overrides
    │   ├── ENCOUNTER_CONFIG — Boss/scaling rules
    │   ├── GAME_BALANCE   — Tuning parameters
    │   ├── LOOT_SYSTEM    — Drop tables
    │   ├── ITEMS          — Item definitions
    │   ├── STATUS_DATA    — Status condition data
    │   ├── TYPE_CHART     — 18×18 type matrix
    │   └── ANIM           — Animation timing
    │
    ├── Core Modules
    │   ├── AudioEngine    — Web Audio synthesizer
    │   ├── API            — PokéAPI integration
    │   ├── Input          — Keyboard/mouse handler
    │   ├── StorageSystem  — localStorage wrapper
    │   └── EncounterManager — Enemy generation
    │
    ├── Game Manager
    │   └── Game           — State machine, screens, party, inventory
    │
    └── Battle Engine
        └── Battle         — Turn system, damage, animations, AI
```

### Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| **Single file** | Zero setup, instant deployment, shareable as one file |
| **No framework** | Pure vanilla JS for minimal overhead and direct DOM control |
| **Object literal modules** | Simple namespace pattern without class boilerplate |
| **Async/await battle engine** | Natural expression of sequential animation chains |
| **PokéAPI dependency** | Access to all 251 Pokémon without bundling sprite assets |
| **Web Audio synthesis** | 8-bit sound effects without audio file dependencies |

---

## 🎯 Debug Mode

Enable debug mode by setting `DEBUG.ENABLED = true` in the JavaScript section:

```javascript
const DEBUG = {
    ENABLED: true,
    PLAYER: {
        ID: 6,           // Force Charizard
        LEVEL: 50,       // Set level
        SHINY: true,     // Force shiny
        MOVES: null,     // Override moves
    },
    ENEMY: {
        ID: 150,         // Force Mewtwo
        LEVEL: 100,      // Set level
        IS_BOSS: true,   // Force boss
    },
    INVENTORY: {
        masterball: 99,
        maxpotion: 99,
    },
    LOOT: {
        FORCE_ITEM: 'ultraball',
    }
};
```

---

## 📊 Technical Details

| Metric | Value |
|--------|-------|
| **Total Lines** | 6,090 |
| **File Size** | ~250 KB |
| **CSS Lines** | ~1,978 |
| **HTML Lines** | ~100 |
| **JS Lines** | ~4,000 |
| **Keyframe Animations** | 50+ |
| **JS Modules** | 7 (AudioEngine, API, Input, StorageSystem, EncounterManager, Game, Battle) |
| **Unique Move Behaviors** | 80+ |
| **Sound Effects** | 27 synthesized SFX |
| **Pokémon Available** | 251 (Gen I + II) |
| **Type Matchups** | Full 18×18 chart |
| **Save Format** | JSON via localStorage |

---

## 🔗 Credits & Dependencies

| Resource | Usage |
|----------|-------|
| [PokéAPI](https://pokeapi.co/) | Pokémon data, stats, moves, sprites |
| [PokeAPI Sprites](https://github.com/PokeAPI/sprites) | Official artwork, icons, front/back sprites |
| [Google Fonts — Press Start 2P](https://fonts.google.com/specimen/Press+Start+2P) | Retro pixel typeface |
| [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) | Synthesized sound effects |

---

## 📄 Documentation

For a detailed structural breakdown of every module, function, constant, and line range in the codebase, see:

📖 **[`agents.md`](./agents.md)** — Complete file structure map

---

## 📜 License

This is a fan project. Pokémon is © Nintendo / Creatures Inc. / GAME FREAK Inc. This project is not affiliated with or endorsed by any of these companies. All Pokémon data is sourced from the open [PokéAPI](https://pokeapi.co/) project.

---

<p align="center">
  <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/gold/25.png" width="56" alt="Pikachu Gold sprite">
  <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/gold/155.png" width="56" alt="Cyndaquil Gold sprite">
  <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/gold/158.png" width="56" alt="Totodile Gold sprite">
  <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/gold/152.png" width="56" alt="Chikorita Gold sprite">
</p>

<p align="center"><em>Built with ❤️ and nostalgia</em></p>
