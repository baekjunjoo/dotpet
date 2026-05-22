# DotPet — Tactile Virtual Pet for Dot Pad

A Tamagotchi-style virtual pet game designed from the ground up for visually impaired users. Care for your pet by feeding, playing, cleaning, and putting them to sleep — using **voice narration**, **rich SFX**, and the **Dot Pad** tactile braille display.

**Inspired by:** [Tamaweb](https://tamawebgame.github.io/) (the classic web Tamagotchi)
**For:** [Dot Pad](https://www.dotincorp.com) — the world's first multi-line braille graphic display
**Built with:** Web Bluetooth / Web Serial, Web Audio, Web Speech API + Azure Neural TTS, liblouis

---

## Concept

Your pet hatches from an egg, then evolves through baby → child → adult stages over a few hours (or days, depending on game speed). How well you care for them determines whether they grow into a **happy adult**, a **normal adult**, or a **sad adult**.

The full game is playable with **voice and SFX alone** — no screen required. The Dot Pad shows a tactile representation of your pet's shape and stat bars so you can feel them growing in real time.

---

## How to play

### Care actions
| Key | Action | Effect |
|---|---|---|
| **F1** or **1** | Feed | +20–35 hunger (depending on need). Wrong food when full = sad pet. |
| **F2** or **2** | Play | Starts mini-game for child/adult; simple bounce for baby. +happiness, -energy. |
| **F3** or **3** | Clean | Restores cleanliness to 100. Children/babies resist a little. |
| **F4** or **4** | Sleep / Wake | Toggle sleep state. Sleeping restores energy. |

### Combos & info
| Keys | Action |
|---|---|
| **F1 + F2** | Announce all stats (Hunger, Happiness, Clean, Energy, Bond) |
| **F3 + F4** | Announce pet info (name, stage, age, days alive) |
| **F1 + F4** | Help — read out key map |

### Mini-games (during Play)
- **Number Guessing** — Pet thinks of a number 1–4. Voice says "higher" / "lower". 4 attempts max.
- **Echo Rhythm** — Pet plays a 3–4 note pattern. Repeat with buttons 1–4. Get it right for happiness boost.

---

## Stats system

Five stats (0–100), all decay over time:

- **Hunger** — increases when fed. Below 30 = pet is hungry.
- **Happiness** — increases when played with. Below 30 = pet is bored.
- **Clean** — restored fully on bath. Below 30 = pet smells.
- **Energy** — restored during sleep. Below 25 = too tired to play.
- **Bond** — long-term care quality. Increases when you act in their best interest, decreases when you neglect or over-feed.

When two or more care stats drop below 30 simultaneously, **Bond** also decays. Critical (Hunger + Clean both below 15) = pet becomes sick.

---

## Evolution

| Stage | Time (Demo speed) | Time (Normal speed) | What's possible |
|---|---|---|---|
| **🥚 Egg** | 0–1 hr | 0–1 day | Listen to heartbeat. No actions yet. |
| **🐣 Baby** | 1–3 hr | 1–3 days | All 4 actions. Play = simple bounce. |
| **🌱 Child** | 3–6 hr | 3–6 days | Mini-games unlock. |
| **🌳 Adult** | 6 hr+ | 6 days+ | Branches based on Bond + care history: |
| ↳ ✨ Happy adult | | | High Bond, mostly positive actions |
| ↳ 🐾 Normal adult | | | Average care |
| ↳ 🌧 Sad adult | | | Low Bond, neglected or over-fed |

Game speed is configurable in Settings: Demo (1 real hr = 1 game day), Normal (1:1), or Slow (3 real days = 1 stage).

---

## Voice / SFX / Dot Pad

### Voice
The pet talks to you through voice narration. Two voice systems:
- **Web Speech API** (default) — uses your OS voice. Free, no setup.
- **Azure Neural TTS** (optional) — sign up at [portal.azure.com](https://portal.azure.com) → Speech resource (free F0 tier, 500K characters/month). Enter the key in Settings.

### SFX
Every action has a distinct sound: feeding chirp, play melody, water splash for cleaning, lullaby for sleep, evolution chord, sad note for needs.

### Dot Pad rendering
- **Stat bars at top/bottom edges** — Hunger + Happiness at top, Clean + Energy at bottom. Length of bar shows current value.
- **Pet shape in center** — Each evolution stage has a distinct tactile silhouette. Sleeping pet has closed eyes (dots 2+5 instead of 1+4 at eye positions).

---

## Accessibility (WCAG 2.1 AA)

- All interactive elements have ARIA labels
- 4.5:1 color contrast minimum
- `role="status"` + `aria-live` for pet announcements
- Keyboard-first design — every action has a key shortcut
- Screen reader compatible (pet speech announced via aria-live)
- Focus-visible outlines on all controls
- Game playable end-to-end with voice + keyboard alone

---

## File structure

```
dotpet-prototype/
├── index.html          # Single-file app (HTML + CSS + JS)
├── dotpad-sdk/
│   └── DotPadSDK-3.0.0.js   # Dot Pad Web SDK (ES module)
└── README.md           # This file
```

Save data is in `localStorage` (`dotpet:save:v1`) — pet persists between sessions and ages while the tab is closed.

---

## Deployment to GitHub Pages

1. Create a new GitHub repository (e.g. `dotpet`):
   ```bash
   cd ~/Documents/Claude/Projects/Dot\ Arcade/dotpet-prototype
   git init
   git add .
   git commit -m "Initial DotPet prototype"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/dotpet.git
   git push -u origin main
   ```
2. In GitHub repo settings → Pages → Source: **main branch / root**
3. Wait ~1 minute for deployment
4. Open `https://YOUR_USERNAME.github.io/dotpet/`

**Important:** Web Bluetooth requires HTTPS. GitHub Pages provides this automatically. For local dev, use `python3 -m http.server` and open via `http://localhost:8000/` (Chrome allows BLE on localhost).

---

## Roadmap (post-MVP)

- More mini-games (memory match, simple maze with the pet)
- Pet "wants" system — pet asks for specific food / activities
- Multiple pets you can switch between (with separate save slots)
- Accessory dressing (hats / scarves) felt on Dot Pad as extra dot patterns
- Online sharing — give your pet's care data a tactile "snapshot" code

---

## Credits

- **Concept inspiration:** Tamaweb by [SamanDev](https://autosam.github.io)
- **Dot Pad SDK:** Dot Incorporation
- **Game by:** Dot Incorporation
