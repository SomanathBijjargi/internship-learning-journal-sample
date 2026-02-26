# Scraping IMDb Top 250 Movies Using Only Browser JavaScript


## 1️⃣ Open Developer Tools
Press:
F12  
OR  
Ctrl + Shift + I  

Go to **Console** tab.
---

## 2️⃣ Select All Movie Items

```javascript
$$(".ipc-metadata-list-summary-item")
```

---
## 3️⃣ Extract Required Data

```javascript
copy(
  $$(".ipc-metadata-list-summary-item").map(item => [
    // Movie Link
    item.querySelector(".ipc-title-link-wrapper")?.href,

    // Title
    item.querySelector(".ipc-title__text")?.textContent,

    // Year
    item.querySelectorAll(".cli-title-metadata-item")[0]?.textContent,

    // Duration
    item.querySelectorAll(".cli-title-metadata-item")[1]?.textContent,

    // Certificate Rating (if exists)
    item.querySelectorAll(".cli-title-metadata-item")[2]?.textContent,

    // IMDb Rating + Votes
    item.querySelector(".ipc-rating-star")?.textContent
  ])
)
```

---

## 4️⃣ What This Uses

- `$$()` → Alias for `document.querySelectorAll()`
- `map()` → Transform each movie element
- `querySelector()` → Select child element
- `querySelectorAll()[index]` → Select specific metadata
- `?.` → Optional chaining (avoids errors)
- `copy()` → Copies result to clipboard

---

## 5️⃣ Convert to CSV / Excel

1. Search: `JSON to CSV converter`
2. Paste copied data
3. Download CSV
4. Open in Excel

---

#  Web Scraping with Python Playwright → Pandas DataFrame

## 🎯 Goal
Scrape a dynamic webpage (NASDAQ stock table) using **Python + Playwright**  
and convert it into a **pandas DataFrame**.

Works even when:
- Table not present in page source
- Data loaded via JavaScript

## step1 : 
```
import request

url = "https://www.nasdaq.com/market-activity/quotes/nasdaq-ndx-index"

User_agent= "Mozilla/5.0 (Macintosh; Intel Mac OS X)"

response = request.get(url, headers={"User_Agent":User_agent})

with open("request.html", "w", encoding="utf-8") as file:
    file.write(response.text)
```


`Problem` : 
HTML returned does NOT contain actual table data  
because page loads data using JavaScript.
---

So requests alone is not enough ❌

Now Install playwright
```
pip install playwright
```

```
from playwright.sync_api import sync_playwright

url = "https://www.nasdaq.com/market-activity/quotes/nasdaq-ndx-index"

USER_AGENT = "Mozilla/5.0 (Macintosh; Intel Mac OS X)"

with sync_playwright() as p:
    # Launch browser
    browser = p.firefox.launch()
    context = browser.new_context(user_agent=USER_AGENT)
    page = content.new_page()
    page.goto(url,timeout=5000)
    content = page.content()

with open("playright.html", "w", encoding="utf-8") as file:
    file.write(content)
```
- Now HTML contains fully rendered table 

```
import pandas as pd

df = pd.read_html("playright.html")[0] # returns a list of data tables
```
## Other Features of playright

Full webpage screenshot saved.
```python
page.screenshot(path="screenshot.png", full_page=True)
```

Wait for Page to Load (if needed)
```python
page.wait_for_timeout(5000)  # wait 5 seconds
```
### Click buttons
```python
page.click("button-selector")
```

### Fill forms
```python
page.fill("#username", "myuser")
```

### Login automation
```python
page.fill("#email", "email")
page.fill("#password", "pass")
page.click("login-btn")
```

