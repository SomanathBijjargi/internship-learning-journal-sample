# LLM Website Scraping

LLM-based website scraping uses Large Language Models (LLMs) to extract structured information from websites without manually writing complex parsing logic.

Traditional scraping tools like BeautifulSoup, Selenium, or Playwright require understanding HTML structure, selectors, and page behavior. With LLMs, much of this work can be automated by prompting the model to interpret the page content and extract relevant data.

## Why Use LLMs for Website Scraping
LLMs simplify scraping tasks by:

* Understanding messy or inconsistent HTML structures
* Extracting data from text-heavy pages
* Converting webpage content directly into structured formats
* Reducing the need for manual CSS/XPath selectors
* Handling semi-structured content automatically

## Typical Workflow

**A common workflow for LLM-based scraping:**

- Fetch the webpage
- Convert HTML to readable text
- Send the content to an LLM
- Ask the model to extract structured information
- Save the results as JSON, CSV, or Markdown

```python
import requests
from bs4 import BeautifulSoup
import openai

url = "https://example.com"
html = requests.get(url).text

soup = BeautifulSoup(html, "html.parser")
text = soup.get_text()

prompt = f"""
Extract the important information from this webpage.

{text}
"""

response = openai.ChatCompletion.create(
    model="gpt-4",
    messages=[{"role":"user","content":prompt}]
)

print(response["choices"][0]["message"]["content"])
```
---
## Chrome Remote Debugging for Web Scraping
Chrome Remote Debugging allows programs or scripts to control a running Chrome browser.
This is useful for web scraping when a website requires login or JavaScript rendering.

Instead of logging in through code, you can log in manually in Chrome, and the script will reuse the same session.

### Step 1: Launch Chrome in Debug Mode 
for `Windows`:
```bash
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir="C:\chrome-debug-profile"
```
This will open a new Chrome window with remote debugging enabled.

### Step 2: Verify Debugging is Running
Open this URL in your browser:
`http://localhost:9222`
You should see a JSON list of open browser tabs.
```json
[
  {
    "id": "page_1",
    "title": "Example Website",
    "url": "https://example.com"
  }
]
```
### Step 3: Login to the Website
In the Chrome window that opened:
* Navigate to the website
* Log in normally
* Your session will now be available for automation.

### Step 4: Connect Using Python (Playwright)
```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.connect_over_cdp("http://localhost:9222")

    context = browser.contexts[0]
    page = context.pages[0]

    page.goto("https://example.com")
    print(page.title())
```
This script:
* Connects to the running Chrome browser
* Uses the logged-in session
* Opens a webpage
* Extracts data

```
Start Chrome in debug mode
        ↓
Login manually
        ↓
Connect using Playwright/Puppeteer
        ↓
Navigate pages and scrape data
```
