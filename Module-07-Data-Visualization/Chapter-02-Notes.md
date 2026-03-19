# Forecasting & Visualization in Excel: Time Series & Correlation Analysis

Data visualization in Excel is a powerful way to explore time series data (such as currencies, indices, and commodities) and extract meaningful insights, trends, and forecasts. 

Below is a step-by-step guide to setting up your forecasting and visualization workspace.

## 1. Data Preparation (Pivot Tables)
Before visualizing, structure your raw data so that you can easily analyze it.
*   Select your data range.
*   Go to **Insert > Pivot Table**.
*   Organize your Pivot Table so that your rows represent individual **Dates** and your columns represent the different **Securities** (e.g., NASDAQ, Australian Dollar, Gold).
*   *Tip:* Ensure your dates are not collapsed into months; expand them so you can see every single day in your time series.

## 2. Visualizing Trends (Sparklines)
Sparklines provide a miniature chart within a single cell to help you quickly spot trends (e.g., dips and growths) across a time period.
*   Create a new column named **Trend**.
*   Click on the cell where you want the trendline to appear.
*   Go to **Insert > Sparklines > Line**.
*   Select the data range for a specific security (e.g., all daily values for the FTSE).
*   Drag the bottom-right corner of the cell down to automatically fill and generate sparklines for all other securities.

## 3. Forecasting Future Values
You can predict the next day's value using Excel's `GROWTH` formula. 
*   **Format Dates as Numbers:** Convert your date column into numbers (which represents the number of days since January 1, 1980) so they increase by 1 for every successive date. 
*   **Determine the Next Date:** Add `1` to the last known date number. This will be your `new_x_value`.
*   **Use the GROWTH Formula:**
    `=GROWTH(known_y_values, known_x_values, new_x_value)`
    *   `known_y_values`: The historical prices/rates of the security.
    *   `known_x_values`: The historical dates (formatted as numbers).
    *   `new_x_value`: The numerical value of tomorrow's date.
*   *Note:* Use absolute references (e.g., `$A$2:$A$90`) for your date ranges so you can safely copy and paste the formula down your list.
*   **Highlight High Growth:** Apply **Conditional Formatting** (Color Scales) to the calculated growth percentages to visually highlight which securities are predicted to increase the most.

## 4. Measuring Fluctuation & Variance
To understand the volatility of your securities, calculate their average and standard deviation.
*   **Average:** `=AVERAGE(data_range)`
*   **Standard Deviation (Variance as a %):** Rather than looking at absolute standard deviation, divide it by the mean to get the variance as a percentage:
    `=STDEV(data_range) / AVERAGE(data_range)`
*   **Spread:** If you prefer a simpler measure or want to avoid outlier distortion, use the difference between the `=MAX(data_range)` and `=MIN(data_range)`.
*   Apply **Conditional Formatting** to easily spot the most volatile securities.

## 5. Analyzing Relationships (Scatterplots & Correlation)
You can visually and quantitatively evaluate how closely two different securities move together (e.g., the Australian Dollar and the Brazilian Riyal).

**Creating a Scatterplot:**
*   Select the columns for the two securities you want to compare.
*   Go to **Insert > Scatterplot**.
*   Right-click a data point on the chart and select **Add Trendline**.
*   Choose a **Linear** trendline and check the boxes for **Display Equation on chart** and **Display R-squared value on chart**.

**Calculating the Correlation Coefficient:**
*   To get the exact correlation percentage without a chart, use the `CORREL` function:
    `=CORREL(array1, array2)`
*   A high percentage (e.g., 95%) indicates a very strong relationship (often seen between neighboring countries' currencies, like the Australian and New Zealand Dollars).

## 6. Generating a Correlation Matrix
To analyze the relationships between *all* pairs of securities at once, generate a Correlation Matrix using the Data Analysis ToolPak.
*   Go to **Data > Data Analysis**. *(If you don't see this, go to File > Options > Add-ins and enable the Analysis ToolPak).*
*   Select **Correlation** and click OK.
*   Select your entire dataset range (ensure data is grouped by columns and check "Labels in first row" if applicable).
*   Choose to output in a new worksheet.
*   **Format the Output:** Convert the resulting decimals into percentages. Apply a **Red-Amber-Green Conditional Formatting** color scale so that highly correlated pairs show up in green, and anti-correlated pairs (negative values) show up in red.