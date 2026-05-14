# CS2 Match Predictor — Project Context

## Goal
A Python ML model that predicts the winner of professional CS2 matches.
Input: two team names → Output: win probability percentage (e.g. "FaZe 73% / Mongolz 27%")

## Current Status
- ✅ Python 3.14 installed, virtual environment set up
- ✅ Dependencies installed (pandas, numpy, scikit-learn, xgboost, lightgbm, matplotlib, seaborn, jupyter)
- ✅ GitHub repo created: cs2-match-predictor
- ✅ Folder structure created (data/raw, data/processed, notebooks, src, models)
- ✅ Kaggle dataset downloaded into data/raw/ (round-by-round CS2 match data from griffindesroches)
- ✅ ClassicalClemi HLTV scraper copied into src/scraping/
- 🔄 Currently on branch: feature/scraper-clemi
- ⏳ Next: install scraper dependencies and test the scraper

## Branch Strategy
```
main        ← stable only, never work directly here
└── dev     ← main working branch
    ├── feature/scraper-clemi   ← current branch
    ├── feature/data-prep
    ├── feature/model-training
    └── feature/predict-cli
```

## Project Structure
```
cs2-match-predictor/
├── data/
│   ├── raw/          # Original CSVs — never edit
│   └── processed/    # Cleaned data output
├── notebooks/        # Jupyter EDA notebooks
├── src/
│   ├── scraping/     # ClassicalClemi HLTV scraper scripts
│   ├── data_prep.py
│   ├── features.py
│   ├── train.py
│   └── predict.py
├── models/           # Saved trained models
├── .env              # API keys — NEVER commit this
├── requirements.txt
└── CLAUDE.md
```

## Data Sources
| Source | Purpose | Status |
|---|---|---|
| Kaggle CSV (griffindesroches) | Historical training data | ✅ Downloaded |
| ClassicalClemi scraper | Fresh match results + team/player stats | 🔄 Integrating |
| PandaScore API (free tier) | Upcoming match schedules | ⏳ Todo |

## Dataset (Kaggle CSV) Columns
`date, event_name, winner_side, team1_name, team2_name, team1_score, team2_score, match_url`
Per map (map1/map2/map3): `start sides, round-by-round winners, map winner, map name`

## ClassicalClemi Scraper — src/scraping/
Five scripts, run in this order:
1. `async_get_team_urls.py` — collects team URLs from HLTV rankings
2. `async_get_team_data.py` — scrapes team stats (ranking, winrate, map winrates per map)
3. `async_get_player_data.py` — scrapes player stats (CT/T side: firepower, clutching, sniping etc.)
4. `async_get_recent_match_urls.py` — collects recent match URLs
5. `async_get_match_data.py` — scrapes match results from those URLs

## Features to Engineer
- Recent win rate (last 10/20 matches)
- Head-to-head record between the two teams
- Map pool strength (win % per map per team)
- CT vs T side win rates
- Team world ranking
- Current win streak

## ML Plan
- Predict: overall match winner (binary classification)
- Baseline: Logistic Regression + Random Forest
- Main model: XGBoost
- Evaluate: log-loss + accuracy on holdout test set

## Tech Stack
- Python 3.14, pandas, numpy
- scikit-learn, xgboost, lightgbm
- matplotlib, seaborn
- camoufox, beautifulsoup4, selenium (scraping)
- Jupyter notebooks for EDA

## Key Decisions Made
- Predicting overall match winner only (not map-by-map)
- Using ClassicalClemi scraper only (not jparedesDS) — more modern Cloudflare bypass
- No hackathon goal — personal project, terminal demo first, GUI later
- PandaScore free tier for upcoming schedules only (paid tier too expensive)
- Old CS:GO Kaggle datasets rejected — CS2 only for accuracy

## Environment
- OS: Windows, using Git Bash in VS Code
- Virtual env: activate with `source venv/Scripts/activate`
- API keys stored in .env file (gitignored)

## Important Notes
- User is new to Python, coming from Java background
- Always explain new concepts clearly
- Keep code well commented
- .env is in .gitignore — never commit API keys
