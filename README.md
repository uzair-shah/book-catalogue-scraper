# Book catalogue scraper

Scrapes every book listed on [books.toscrape.com](https://books.toscrape.com), a practice site built for web scraping, and stores the details in a SQLite database you can query.

📄 **Full write-up with charts:** [uzair-shah.github.io/projects/book-scraper.html](https://uzair-shah.github.io/projects/book-scraper.html)

## What it does

1. Visits each catalogue page in turn and stops when a page no longer exists, so it doesn't need to know the page count.
2. For every book, collects:
   - **name**: the full title
   - **price**: in £, stored as a number
   - **in_stock**: the availability text
   - **rating**: converted from words ("One" to "Five") to 1–5
   - **link**: the full URL of the cover image
3. Waits half a second between pages so it doesn't overload the site.
4. Saves everything to `books.db`, in a table called `books`.
5. Runs a sample query (books under £20, cheapest first) and prints the first few rows.

## The data

`books.db` is included, so you can query it without running the scraper.

- **1,000 books** from 50 pages (20 per page)
- **Prices** from £10.00 to £59.99, average £35.07
- **196 books under £20**, 42 of them with 5 stars
- Every book is listed as "In stock"
- One title appears twice at different prices ("The Star-Touched Queen")

## Files

| File | What it is |
|---|---|
| `scraper.py` | The finished script: scrapes all pages, saves to `books.db`, prints the sample query |
| `scraper.ipynb` | Notebook that builds the scraper step by step (one book, then one page, then all pages), with saved output from a full run |
| `books.db` | The SQLite database from a run: one `books` table, 1,000 rows |
| `git-cheat-sheet-education.pdf` | A Git command reference, not part of the scraper |
| `.vscode/settings.json` | Editor setting for VS Code |

## Run it

```bash
pip install requests beautifulsoup4 pandas
python scraper.py
```

It prints a line for each page as it goes, then the sample query results. `sqlite3` is built into Python, so it needs no install.

## Query the data yourself

```python
import sqlite3, pandas as pd

with sqlite3.connect("books.db") as conn:
    print(pd.read_sql("SELECT * FROM books WHERE rating = 5", conn))
```
