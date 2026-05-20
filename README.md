# 🎯 Quote Guessing Game

> A Jupyter Notebook game that scrapes real quotes from the web and challenges you to guess the author — with progressive hints to keep things fair.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Demo](#demo)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [How the Game Works](#how-the-game-works)
- [Bug Fixes](#bug-fixes)
- [Data Source](#data-source)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**Quote Guessing Game** is an interactive Jupyter Notebook project that:

1. Web-scrapes all quotes from [quotes.toscrape.com](http://quotes.toscrape.com) across every paginated page.
2. Caches the results locally to a CSV file for instant reuse on subsequent runs.
3. Presents a random quote and challenges the player to name the author within **3 guesses**, unlocking progressive hints after each wrong answer.

This project demonstrates practical skills in web scraping (`requests` + `BeautifulSoup`), CSV data persistence, rich notebook output with `IPython.display`, and interactive terminal-style gameplay.

---

## Features

| Feature | Description |
|---|---|
| 🌐 Web Scraper | Paginates through all pages of quotes.toscrape.com automatically |
| 💾 CSV Cache | Saves scraped data locally — no re-scraping on repeat runs |
| 🎮 Interactive Game | 3-guess quiz loop with clean, styled output |
| 💡 Progressive Hints | Bio info → first-name initial → last-name initial |
| 🎨 Styled Output | HTML-rendered quote cards and coloured feedback via `IPython.display` |
| 📊 Data Explorer | Bonus section with author frequency stats and dataset preview |

---

## Demo

```
🎯 Guess the author of this quote:

  ┌─────────────────────────────────────────────────────────────┐
  │  "The world as we have created it is a process of our       │
  │   thinking. It cannot be changed without changing our        │
  │   thinking."                                                 │
  └─────────────────────────────────────────────────────────────┘

Your guess [3 guesses left]: Ernest Hemingway
✗ Not quite.
💡 Hint: The author was born on March 14, 1879 in Ulm, Germany.

Your guess [2 guesses left]: Albert Einstein
🎉 Correct! The author is Albert Einstein.
```

---

## Project Structure

```
quote-guessing-game/
│
├── Quote_Guessing_Game_Enhanced.ipynb   # Main notebook (scraper + game + explorer)
├── quotes.csv                           # Auto-generated cache (created on first run)
└── README.md
```

---

## Requirements

- Python **3.9+**
- Jupyter Notebook or JupyterLab

### Python Dependencies

```
requests
beautifulsoup4
ipython
```

> All other imports (`csv`, `time`, `random`, `pathlib`, `collections`) are part of the Python standard library.

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/your-username/quote-guessing-game.git
cd quote-guessing-game
```

### 2. Create and activate a virtual environment (recommended)

```bash
python -m venv venv

# macOS / Linux
source venv/bin/activate

# Windows
venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install requests beautifulsoup4 ipython
```

### 4. Launch Jupyter

```bash
jupyter notebook
```

Then open `Quote_Guessing_Game_Enhanced.ipynb`.

---

## Usage

Run the notebook cells **top to bottom**:

| Cell | Section | What it does |
|---|---|---|
| 1 | Imports | Loads all required libraries |
| 2 | Configuration | Sets `BASE_URL`, CSV path, and `MAX_GUESSES` |
| 3 | Scraper | Scrapes quotes (or loads from cache if CSV exists) |
| 4 | Helpers | Defines `fetch_bio_hint`, `get_hint`, and display utilities |
| **5** | **Game** | **Run this cell to start playing — re-run for a new quote** |
| 6 | Explorer | Prints author frequency stats and a dataset preview |

> **Tip:** Only Section 5 needs to be re-run between rounds. All scraped data is retained in memory for the full session.

---

## How the Game Works

```
         ┌──────────────────────────────┐
         │   Random quote is displayed  │
         └──────────────┬───────────────┘
                        │
              ┌─────────▼──────────┐
              │   Player guesses   │  (3 attempts)
              └────────┬───────────┘
                       │
          ┌────────────▼─────────────┐
          │  Correct?                │
          │  YES → 🎉 Win message    │
          │  NO  → remaining -= 1    │
          └────────────┬─────────────┘
                       │
         ┌─────────────▼──────────────────────┐
         │  Hint unlocked based on remaining: │
         │  2 left → birthdate & birthplace   │
         │  1 left → first-name initial       │
         │  0 left → last-name initial        │
         └─────────────┬──────────────────────┘
                       │
              ┌────────▼──────────┐
              │  0 guesses left?  │
              │  YES → 😞 Reveal  │
              └───────────────────┘
```

---

## Bug Fixes

Five bugs were identified and corrected from the original version:

| # | Bug | Root Cause | Fix Applied |
|---|-----|-----------|-------------|
| 1 | Pagination silently broke | `soup.find(_class="next")` — `_class` is not a valid BeautifulSoup keyword argument | Changed to `class_="next"` |
| 2 | Double-slash in scraped URLs | `BASE_URL` ended with `/` and relative URLs started with `/` | Removed trailing slash from `BASE_URL` |
| 3 | Bio hint was unreachable | `MAX_GUESSES = 2` but hint thresholds were coded for values 3, 2, and 1 | Set `MAX_GUESSES = 3` |
| 4 | Correct guess rejected on case mismatch | Comparison used `guess == quote["author"]` inside a `.lower()` processing block | Standardised all comparisons to `.lower()` on both sides |
| 5 | Game-over message never displayed | Message was inside an `else` branch that the loop logic never reached | Moved the game-over message outside the loop |

---

## Data Source

All quotes are scraped from **[quotes.toscrape.com](http://quotes.toscrape.com)** — a sandbox website provided specifically for practising web scraping. It contains ~100 quotes from authors such as Albert Einstein, Mark Twain, J.K. Rowling, and more.

A polite `1-second` delay (`SLEEP_SECS = 1`) is set between page requests to avoid hammering the server.

---

## Contributing

Contributions are welcome! Here are some ideas for extending the project:

- [ ] Difficulty modes (Easy / Hard — control which hints are available)
- [ ] Score tracking across multiple rounds
- [ ] Tag-based filtering (play only philosophy quotes, etc.)
- [ ] A `tkinter` or Streamlit GUI version
- [ ] Unit tests for the scraper and hint logic

To contribute:

```bash
# 1. Fork the repository
# 2. Create a feature branch
git checkout -b feature/your-feature-name

# 3. Commit your changes
git commit -m "Add: brief description of change"

# 4. Push and open a Pull Request
git push origin feature/your-feature-name
```

---

## License

This project is licensed under the **MIT License**. See [LICENSE](LICENSE) for details.

---

<p align="center">
  Made with Python 🐍 and <a href="http://quotes.toscrape.com">quotes.toscrape.com</a>
</p>
