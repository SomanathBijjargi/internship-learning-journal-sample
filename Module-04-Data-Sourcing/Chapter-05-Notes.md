# Scheduled Scraping with GitHub Actions

**Scheduled scraping with GitHub Actions means running a scraping script automatically at a fixed time using GitHub’s automation system.**

GitHub Actions provides an excellent platform for running web scrapers on a schedule. 

Here’s a sample scrape.py that scrapes the IMDb Top 250 movies using httpx and lxml

**Scraping Script (scrape.py)**
```python
import json
import httpx
from datetime import datetime, UTC
from lxml import html
from typing import List, Dict

def scrape_imdb() -> List[Dict[str, str]]:
    """Scrape IMDb Top 250 movies using httpx and lxml.
    Returns:
    List of dictionaries containing movie title, year, and rating.
    """
    headers = {"User-Agent": "Mozilla/5.0 (compatible; IMDbBot/1.0)"}
    response = httpx.get("https://www.imdb.com/chart/top/", headers=headers)
    response.raise_for_status()
    tree = html.fromstring(response.text)
    movies = []
    
    for item in tree.cssselect(".ipc-metadata-list-summary-item"):
        title = (
            item.cssselect(".ipc-title__text").text_content() 
            if item.cssselect(".ipc-title__text") else None
        )
        year = (
            item.cssselect(".cli-title-metadata span").text_content() 
            if item.cssselect(".cli-title-metadata span") else None
        )
        rating = (
            item.cssselect(".ipc-rating-star").text_content() 
            if item.cssselect(".ipc-rating-star") else None
        )
        if title and year and rating:
            movies.append({"title": title, "year": year, "rating": rating})
            
    return movies

# Scrape data and save with timestamp
now = datetime.now(UTC)
with open(f"imdb-top250-{now.strftime('%Y-%m-%d')}.json", "a") as f:
    f.write(json.dumps({"timestamp": now.isoformat(), "movies": scrape_imdb()}) + "\n")
```

### GitHub Actions Workflow (.github/workflows/imdb-top250.yml)

Here’s a sample YAML configuration that runs the scraper daily and saves the scraped data directly to the repository

```yml
name: Scrape IMDb Top 250
on:
  schedule:
    # Runs at 00:00 UTC every day
    - cron: "0 0 * * *"
  workflow_dispatch: # Allow manual triggers

jobs:
  scrape-imdb:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      - name: Install uv
        uses: astral-sh/setup-uv@v5
      - name: Run scraper
        run: |
          uv run --with httpx,lxml,cssselect python scrape.py
      - name: Commit and push changes
        run: |
          git config --local user.email "github-actions[bot]@users.noreply.github.com"
          git config --local user.name "github-actions[bot]"
          git add *.json
          git commit -m "Update IMDb Top 250 data [skip ci]" || exit 0
          git push
```
**Note** : `A .yml file is used to configure and automate tasks, such as running workflows, setting up servers, or defining application settings.`

#  Making Open Data Useful:
### 1. The Core Pipeline: Fetch → Parse → Aggregate
* **Fetch:** Grab the raw pages or API responses exactly as they are served. Since government pages move and data often disappears, it is crucial to keep every original file.
* **Parse:** Convert the raw HTML, PDFs, or odd spreadsheets into tidy JSON. Keep transformations minimal at this stage so you can easily revisit decisions later.
* **Aggregate:** Join and normalize the parsed data into a single analyzable table, such as a CSV or a SQLite file, which will be the primary file others actually use.

### 2. Advanced Scraping and Extraction Techniques
* **Network Tab and HAR Files:** When scraping complex sites (like those with ASP.NET state or tricky forms), use the browser's Network tab. Export the requests as a HAR file, then use an LLM to generate a scraper (e.g., using httpx or Playwright) that replays those exact steps to grab the raw responses.

* **LLMs for Extraction, Not Insights:** Use Large Language Models to classify text, normalize messy strings, and extract structured fields into a strict JSON schema. For instance, this technique was used in CBFC Watch to turn 100,000 free-text "cut notes" into analyzable columns at scale.
    - Best Practice: Provide a JSON schema, prompt the LLM to output valid JSON only, validate every response, and manually spot-check samples.

### 3. Product and Architecture Principles: 
* **Linkability is Everything:** "If you cannot link to it, it will not exist". URLs should be treated as part of the product by encoding every filter and view into the query string. Update the URL (window.history.replaceState) when a user changes filters so the exact view can be shared and restored on load.

* **Browser-First, Static Apps:** Prefer static apps that run fully in the browser and "live forever". They are fast, cost nothing to host (using GitHub Pages or Netlify), and are easy to archive. Tools like DuckDB-WASM or sql.js can be used for client-side querying of millions of rows.

* **Simple Search over Checkboxes:** Avoid walls of toggles. Instead, provide a simple search box backed by a fast index and a tiny query parser (e.g., allowing syntax like field:value or year>=2021).

* **Design for Beauty and Craft:** A well-composed narrative page beats a generic dashboard. Meaningful hover states, clear typography, and mobile-friendly layouts are essential for impact.

### 4. Serving Different Audiences
Your website is just one view of the data. You must design multiple entry points for different types of users:
* A simple search/browse view for curious readers.
* Downloadable CSVs and code for developers.
* Reproducible notebooks (in Python, R, or SQL) for analysts and reporters to rebuild figures.

**Documentation is a first-class feature**. Always include a clear data dictionary with one-line explanations and allowed values, alongside example queries and links.

### 5. Solving Common Data Headaches
* **Spelling Variants:** Normalize and classify using an LLM schema
* **Cryptic Codes:** Build lookup tables and ship them with the repo to show values in plain words
*** Scattered Files:** Write one parser per file type, standardize, and append into a single table
* **Maze-like Portals:** Recreate the smallest, simplest UI that answers realistic questions and make it shareable via permalinks

### 6. The Recommended Tech Stack
For students and developers looking to ship open-data projects, the recommended minimal, modern stack includes:
* Scraping: DevTools Network + HAR, Playwright or httpx.
* Cleaning: Python/R (Pandas/Dplyr) and DuckDB for joins.
* LLM-ops: Small batch processing with strict JSON schemas and validators.
* Search: Typesense or a small client-side index with a tiny parser.
* Web/Visualization: Static sites (GitHub Pages/Netlify) with MapLibre or light charting libraries.

### 7. Diagram Chasing Case Studies
These principles have been successfully applied across several distinct projects
* **CBFC Watch:** A searchable archive of film modifications with deep links and open data.
* **India Time Use Explorer:** A static, browser-only explorer for a massive national survey.
* **BLR Water Log:** A static map visualizing natural drainage and flood risks in Bengaluru.
* **Votes in a name?:** A narrative analysis investigating namesake political candidates.
* **Who is my neta?:** A civic tool that joins multiple public sources (affidavits, legislative activity, geography) into a unified view.