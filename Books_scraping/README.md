# Scraping — Books to Scrape

## Overview
- Website: https://books.toscrape.com/ (the link that was in the Word document on Drive)
- The catalogue is paginated (`catalogue/page-1.html`, `page-2.html`, ...), so the script loops over pages until a page returns no books. That turned out to be **50 pages**.
- For every book I extracted:
  - `category`
  - `title`
  - `price` (numeric, in £)
  - `in_stock` (True / False)
- If a field could not be found, I put `unknown` in its place, as required.
- Result: `books.csv` with **1,000 books**, 4 columns, 50 different categories, and no missing/unknown values (every field was found for every book).

## Files
- `scrape_books.ipynb` — the notebook version, step by step
- `scrape_books.py` — the same code as a runnable script
- `books.csv` — the final output

## Problems I faced and how I solved them
1. **The category is not on the listing page.**
   The catalogue pages only show title, price and availability. I opened each book's own page and read the category from the breadcrumb (Home > Books > Category > Title).
2. **Pagination does not say how many pages there are.**
   Instead of hard-coding a number, I looped page by page and stopped when a page returned 404 / contained no books.
3. **The price comes with a currency symbol (£51.77).**
   I cleaned it with a regular expression that keeps only the digits and the dot, then converted it to `float`.
4. **Titles on the listing page are cut off (`...`).**
   I used the full title stored in the link's `title` attribute instead of the visible text.
5. **Visiting 1,000 book pages one by one is slow.**
   In the script version I fetched the detail pages in parallel (ThreadPoolExecutor), which brought the run down to about a minute. The notebook version does the same thing sequentially so the steps are easy to follow.

## How to run
```bash
pip install requests beautifulsoup4 pandas
python scrape_books.py   # writes books.csv
```
