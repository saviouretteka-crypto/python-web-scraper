
#  News Web Scraper

A production-style web scraper built with **Requests** and **BeautifulSoup** that extracts headlines, authors, timestamps, and summaries from news pages — then saves them to CSV with full error handling and retry logic.

---

## Skills Demonstrated

| Skill | Usage |
|---|---|
| **Requests** | HTTP GET with custom headers, timeout, and 3-attempt retry logic |
| **BeautifulSoup** | CSS selector-based extraction of structured article data |
| **Data Extraction** | `Article` dataclass cleanly models each scraped record |
| **File Handling** | CSV output with auto-rename to prevent overwriting existing files |
| **Error Handling** | Catches `Timeout`, `ConnectionError`, `HTTPError`, `PermissionError` separately |

---

## Features

-  Extract headlines — headline, author, timestamp, summary, category, URL
-  Save to CSV — structured output with headers, timestamped filenames
-  Retry logic — 3 automatic retries with delay on network failures
-  Error handling — graceful handling of HTTP errors, timeouts, malformed pages
-  Offline testing — built-in mock HTML page for development without network access

---

## Installation

```bash
# Clone the repository
git clone https://github.com/your-username/news-web-scraper.git
cd news-web-scraper

# Install dependencies
pip install requests beautifulsoup4
```

---

## Usage

**Local (CLI):**
```bash
# Test with included mock HTML — no internet needed
python news_scraper.py --file mock_news.html

# Scrape a live URL
python news_scraper.py --url https://example-news-site.com

# Custom output filename
python news_scraper.py --url https://... --output my_headlines.csv
```


```python
# Edit the CONFIG block at the top of news_scraper_kaggle.py
SOURCE_URL  = ""        # paste a live URL, or leave empty
USE_MOCK    = True      # True = use built-in sample data
OUTPUT_FILE = "/kaggle/working/headlines.csv"

# Then run the cell — no argparse, no CLI flags needed
```

---

## Project Structure

```
news-web-scraper/
│
├── news_scraper.py           # Main scraper (local CLI version)
├── news_scraper_kaggle.py    # Kaggle/Colab version (CONFIG block, no argparse)
├── mock_news.html            # Sample HTML page for offline testing
├── headlines.csv             # Example output (generated on run)
└── README.md
```

---

## Sample Output

Terminal:
```
============================================================
  NEWS WEB SCRAPER
============================================================
21:17:45  INFO      Using built-in mock data (USE_MOCK = True)

  Parsing HTML …
21:17:45  INFO      Found 8 article cards
21:17:45  INFO      [01] Python 4.0 Released with Major Performance Improvements
21:17:45  INFO      [02] AI Chip Demand Drives Record Semiconductor Revenue
...

  Saving to CSV …
21:17:45  INFO      ✔  Saved 8 articles → headlines.csv  (1.9 KB)

============================================================
  SCRAPE REPORT  —  8 articles extracted
============================================================

  Categories:
    Technology      ▪▪  (2)
    Business        ▪▪  (2)
    AI              ▪   (1)
    Security        ▪   (1)

  Headlines:
    01. [Technology] Python 4.0 Released with Major Performance Improvements
        Jane Smith  ·  2024-06-15 08:30
    02. [Business] AI Chip Demand Drives Record Semiconductor Revenue
        Tom Richards  ·  2024-06-15 09:15
```

CSV output (`headlines.csv`):
```
headline,author,timestamp,summary,category,url,scraped_at
Python 4.0 Released...,Jane Smith,2024-06-15 08:30,The Python Software...,Technology,/article/1,2024-06-15 21:17:45
AI Chip Demand Drives...,Tom Richards,2024-06-15 09:15,Global semiconductor...,Business,/article/2,2024-06-15 21:17:45
```

---

## Adapting to a Real News Site

Update the CSS selectors inside `parse_articles()` to match your target site's HTML:

```python
# Current selectors (works with mock_news.html)
cards     = soup.select("article.news-item")
link_tag  = card.select_one(".story-link")
author    = _text(card, ".author",    prefix="By ")
timestamp = _text(card, ".timestamp")
summary   = _text(card, ".summary")
category  = _text(card, ".category")
```

Use browser DevTools (`F12 → Inspect`) to find the right class names for any site.

> ⚠️ Always check a site's `robots.txt` and Terms of Service before scraping.

---

## Dependencies

```
requests>=2.28
beautifulsoup4>=4.12
```
