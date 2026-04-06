# GRID ORACLE — F1 Race Engineer Agent

> *"Box, box, box. Undercut window is open. 78% confidence."*

An LLM-based Formula 1 pit wall race engineer built on the **OpenAI Responses API**. You play the Team Principal. The agent plays your race engineer — making live strategy calls, pushing back on bad decisions, and updating its recommendations as the race evolves.

Built for **COMP47980 Generative AI and Language Models**, University College Dublin.

---

## What It Does

You type questions like a team principal on the pit wall. The agent responds in **radio style** with:
- A clear strategic call
- A **confidence percentage**
- Reasoning that **cites the specific knowledge file** that informed it

It tracks tyre age, gaps to rivals, weather, and safety car status across a full multi-turn conversation. As conditions change, the recommendation changes. If you make a bad call, it tells you.

---

## Architecture

### File Search (RAG)
Four curated knowledge files, automatically uploaded to an OpenAI vector store:
- `circuits_2026.txt` — circuit strategy knowledge base (pit windows, undercut viability, DRS zones)
- `drivers_2026.txt` — driver profiles and radio tone guidance
- `history_2026.txt` — 2024 race results, safety car statistics, undercut success rates

The agent **must cite which file section** informed every recommendation.

### Code Interpreter
Used for computations requiring guaranteed numerical accuracy:
- Undercut window modelling with visual charts
- Monte Carlo championship simulations

### 7 Function Callbacks

| Function | Description |
|---|---|
| `get_live_lap_data` | OpenF1 API — live lap times |
| `get_tyre_status` | OpenF1 API — current tyre compound and age |
| `get_gap_to_rivals` | OpenF1 API — real-time intervals |
| `get_circuit_weather` | OpenWeatherMap — live conditions; rain >40% at Spa flips strategy from 1-stop to 2-stop |
| `add_race_to_calendar` | Writes race events to SQLite |
| `predict_rival_pit_lap` | Tyre degradation model, updates undercut probability in dashboard |
| `calculate_championship_scenario` | Guaranteed Python arithmetic for points scenarios |

---

## Why Not Just Use ChatGPT?

| Capability | ChatGPT | Grid Oracle |
|---|---|---|
| Call OpenF1 during a conversation | ✗ | ✓ |
| Fetch live weather and flip strategy on rain probability | ✗ | ✓ |
| Accurate championship arithmetic on edge cases | ✗ | ✓ |
| Memory of lap number and tyre history across turns | ✗ | ✓ |

---

## Setup

1. Open `GRID_ORACLE_v2_FINAL.ipynb` in **Google Colab**
2. Add `OPENAI_API_KEY` to Colab Secrets (key icon in left sidebar)
3. Add `OPENWEATHER_API_KEY` to Colab Secrets (free at [openweathermap.org](https://openweathermap.org) — optional, falls back to simulated weather)
4. Run cells **top to bottom, one at a time**
5. ⚠️ **Do not use Run All** — the live chat cell blocks on `input()`

---

## Examples Included

1. Pit call with driver-adapted radio tone
2. Weather flips strategy at Spa
3. Agent changes its mind mid-conversation
4. Code interpreter — undercut chart and Monte Carlo simulation
5. Safety car exploitation and calendar write
6. Agent pushes back on a bad call
7. Hamilton vs Verstappen radio tone comparison
8. Post-race self-audit

---

## Cost Note

The **code interpreter** is the most expensive tool per the course brief. It only fires in Example 4.

Run the **cleanup cell** after testing to delete the vector store and avoid ongoing OpenAI storage charges.

---

## Tech Stack

- Python 3.10+
- OpenAI Responses API (GPT-4o)
- OpenF1 API (live telemetry)
- OpenWeatherMap API
- SQLite
- Google Colab

---

## Course

COMP47980 — Generative AI and Language Models
University College Dublin
