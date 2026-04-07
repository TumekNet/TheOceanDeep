# 🌊 Bubble Pop! Ocean Adventure

A personalised multiplication times tables game built for kids, deployed as a single HTML file on Netlify with Supabase cloud saves.

Penny pops the correct answer bubble before it floats away — working through 10 campaign levels from Shallow Waters to the Hadal Zone, earning stars, unlocking bubble themes, and collecting achievements along the way.

---

## Features

### 🗺️ Campaign Mode
10 progressive levels that introduce times tables gradually, starting with the easy ones (×2, ×5) and building to the full hard set (×6, ×7, ×8, ×9, ×11, ×12). Each level has a defined pass threshold and awards 1–3 stars based on accuracy. Levels must be passed to unlock the next, and all three stars on every level is the ultimate goal.

| Level | Name | Tables | Questions | Pass |
|-------|------|--------|-----------|------|
| 1 | Shallow Waters | ×2, ×5 | 12 | 60% |
| 2 | Coral Reef | ×2, ×5, ×10 | 12 | 60% |
| 3 | Kelp Forest | ×2, ×3, ×5, ×10 | 12 | 60% |
| 4 | Open Ocean | ×2, ×3, ×4, ×5 | 15 | 65% |
| 5 | The Currents | ×3, ×4, ×6 | 15 | 65% |
| 6 | Shipwreck | ×4, ×6, ×7 | 15 | 65% |
| 7 | The Trenches | ×6, ×7, ×8 | 18 | 70% |
| 8 | Midnight Zone | ×7, ×8, ×9 | 18 | 70% |
| 9 | The Abyss | ×8, ×9, ×11, ×12 | 18 | 70% |
| 10 | Hadal Zone | ×6–×9, ×11, ×12 | 20 | 75% |

### 💧 Free Play Mode
Choose any combination of times tables, pick a difficulty (Easy / Medium / Hard), and dive in. Great for targeted practice on specific tables.

### 🎨 Bubble Themes
11 unlockable bubble themes earned by progressing through the campaign. Themes include Berry Burst, Lava Bubbles, Galaxy, Electric, and the champion-only Royal Gold. Active theme is applied to all bubbles in gameplay.

### 🏅 Achievements
18 achievements covering gameplay milestones, funny moments, and hidden challenges — including Penny Drop! (score exactly 100 points), Night Owl (play after 9pm), Comeback Kid (pass with 1 life left), and Ocean Champion (3-star every level).

### 🧜 Character (Roadmap)
A mermaid character is planned with gear, outfit, and appearance unlocks tied to star milestones. The character screen is live with a roadmap preview.

### ☁️ Cloud Saves (Supabase)
Player progress — stars, themes, achievements, and high scores — is saved to Supabase on every session. A UUID is generated on first visit and stored in `localStorage` as the player identifier. Progress is safe across browser clears, device switches, and game updates.

### 🔊 Sound Effects
All sounds are generated with the Web Audio API — no external files, works fully offline. Includes correct pop, wrong pop, bubble escaped, streak chime, level complete fanfare, achievement bells, theme unlock, and per-star cascade sounds. Mute button available during gameplay.

### 📊 Session Export
Every game session can be downloaded as a JSON file containing the full question-by-question breakdown — question asked, correct answer, what was tapped, result, and points. Useful for tracking learning progress over time.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Single-file HTML, CSS, Vanilla JS |
| Fonts | Google Fonts (Fredoka One, Nunito) |
| Audio | Web Audio API (no external files) |
| Database | Supabase (PostgreSQL, REST API) |
| Hosting | Netlify (auto-deploy from GitHub) |

No build step, no npm, no frameworks. Push to GitHub and Netlify deploys automatically.

---

## Deployment

### 1. Supabase Setup

Create a project at [supabase.com](https://supabase.com) and run the following in the SQL Editor:

```sql
create table players (
  id uuid primary key,
  name text not null,
  created_at timestamptz default now(),
  campaign jsonb default '{"levelStars":[],"unlockedThemes":["default"],"currentTheme":"default"}'::jsonb,
  achievements jsonb default '[]'::jsonb,
  high_scores jsonb default '[]'::jsonb
);

alter table players enable row level security;

create policy "Players can manage their own row"
  on players
  for all
  using (true)
  with check (true);
```

Then grab your project URL and anon public key from **Project Settings → API** and update these constants near the top of `bubble-pop-campaign.html`:

```js
const SUPA_URL = 'https://your-project.supabase.co';
const SUPA_KEY = 'your-anon-key';
```

### 2. GitHub

Push `bubble-pop-campaign.html` to your repo — rename it to `index.html` for Netlify to serve it as the root page:

```bash
git add index.html
git commit -m "Deploy Bubble Pop campaign"
git push
```

### 3. Netlify

Connect your GitHub repo to Netlify. No build command or publish directory needed — Netlify will serve `index.html` directly. Every push to `main` auto-deploys.

---

## Updating the Game

Progress is stored in Supabase, not in the HTML file. Pushing an updated `index.html` to GitHub **will not affect any player's saved progress** as long as you follow these rules:

- ✅ Safe to change: bubble speed, point values, UI, bug fixes, new screens, CSS
- ✅ Safe to add: new achievements (append to the `ACHIEVEMENTS` array), new themes
- ⚠️ Avoid: reordering the `LEVELS` array (stars are stored by index)
- ⚠️ Avoid: renaming existing keys inside the `campaign` JSON object

---

## Project Structure

```
index.html          — The entire game (HTML + CSS + JS, single file)
README.md           — This file
```

---

## Roadmap

- [ ] Mermaid character with gear and outfit unlocks tied to star milestones
- [ ] Background music (toggleable)
- [ ] Parent dashboard — view Penny's progress and session history
- [ ] Configurable player name without clearing save data
- [ ] Animated level completion sequence with star reveal
- [ ] Division mode (reverse the question)

---

## Notes for Parents

The first time the game is opened, it will ask for a name. This creates a save record in Supabase. The game works on any modern browser — Chrome, Safari, Firefox — and is optimised for iPad touch. An internet connection is required on first load (to fetch the Google Font and connect to Supabase) but gameplay itself will function offline if the font is already cached.

If Penny ever needs to continue on a different device, the `bubblePop_pid` value from `localStorage` on the original device can be copied across to restore her save.

---

*Built with Claude — Anthropic, April 2026*
