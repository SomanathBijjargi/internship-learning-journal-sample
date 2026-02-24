

# Week 4 Session 1: APIs, Geocoding, and Data Extraction

## 1. Extracting Backend APIs via Browser Network Tools
Instead of web scraping static pages, you can often find the hidden APIs that websites use to load dynamic data (like search suggestions). 

*   **Finding the API:** You can capture these backend calls by opening your browser's Developer Tools, going to the **Network tab**, and triggering an action (like typing a letter into a search bar). This allows you to intercept the Request URL being called by the website's JavaScript.
*   **Understanding URLs:** A standard API URL consists of the host (server URL), the path (like folders), and query arguments (key-value pairs separated by `&` and initiated by a `?`). When manually passing queries, you may need to format strings to handle spaces and commas (e.g., replacing them with `%20` or `%2C`).
*   **Fetching Data:** Once you have the target URL, you can use Python's `requests.get(URL)` method to fetch the data.
*   **Handling Malformed JSON Responses:** Sometimes, hitting an API (like Wikipedia's internal search API) returns text that contains a JSON object wrapped inside other characters or syntax errors. To solve this, you can perform manual string manipulation:
    *   Use `.find()` to locate the start and end indices of the actual JSON string.
    *   Slice the response text to extract only the valid JSON portion.
    *   Convert the extracted string into a Python dictionary using `json.loads()`.

## 2. Geocoding with Geopy (Nominatim)
The session extensively covers the `Nominatim` geocoder from the `geopy` library, which allows you to convert between addresses and geographic coordinates.

*   **Setup and User Agent:** To initialize Nominatim, you must provide a `user_agent`. **It is crucial to use lowercase letters and no spaces** (e.g., `user_agent="abc"`), otherwise, the API might reject the connection.
*   **Forward Geocoding (`app.geocode`):** This function takes a text address (e.g., "IIT Madras") and returns an object containing the exact address, **latitude**, and **longitude**. You can also access the raw dictionary of data using `.raw`.
*   **Reverse Geocoding (`app.reverse`):** This function does the opposite; it takes a tuple of `(latitude, longitude)` and returns the physical address.
*   **Batch Geocoding with Pandas:** When processing multiple addresses at once, you can load them into a Pandas DataFrame and apply the geocoding function across the rows. 
    *   **Handling Missing Data:** If an address is invalid, the geocoder returns `None`. You must write logic (often using `lambda` functions) to handle `None` values to avoid throwing errors when trying to extract latitude or longitude.
*   **Rate Limiting:** Nominatim is a free, open-source API and restricts how fast you can make requests. If you make too many requests in a loop, it will block you and throw a warning. **You must use Geopy's `RateLimiter` with at least a 1-second delay** (`min_delay_seconds=1`) or use `time.sleep(1)` to respect the API limits.
*   **Language Formatting:** You can change the language of your geocoding results by passing a `language` parameter (e.g., using `'hi'` for Hindi or `'en'` for English).

## 3. The Wikipedia Python Package
Instead of parsing Wikipedia's raw HTML manually, you can use the `wikipedia` Python package (`pip install wikipedia`) to interact with the site programmatically.

*   **Searching Topics:** Use `wikipedia.search("topic")` to return a list of matching article titles. You can limit the number of results returned.
*   **Generating Summaries:** The `wikipedia.summary()` function returns a text overview of a specific topic. You can specify the exact length of the summary by passing the `sentences` parameter (e.g., `sentences=1`).
*   **Extracting Page Elements:** Using `wikipedia.page("topic")`, you generate a page object. From this object, you can easily extract page-specific data like the **URL, references, title, images, and HTML content** (which is particularly useful for extracting Wikipedia tables using Pandas).