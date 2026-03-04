# Data Cleaning in Excel
 Excel is an effective tool for cleaning data, especially when the data size is small and the user is familiar with these basic functionalities

### 1. Find and Replace

You can quickly remove or replace unwanted text across your dataset.

- Press Ctrl + H to open the Find and Replace dialog box
- In the Find what field, enter the exact term you are looking for 
- Leave the Replace with field completely blank if you simply want to delete the term without substituting anything
- Click Replace All to instantly clean the data across the sheet

### 2. Changing Data Formats
- Highlight the relevant columns
- Navigate to the data type dropdown tab at the top of the Excel window
- Change the format from General to Number. This will convert the values to a numerical data type, standardizing them with up to two decimal places.

### 3. Removing Extra Spaces (TRIM Function)
Inconsistent spacing, such as extra spaces before words or around hyphens, can be cleaned up using Excel's built-in TRIM function

- Insert a new column next to the data you want to clean
- Use the following formula in the first cell:

`(For example, if cleaning data in cell D2, you would use =TRIM(D2))`

- Drag and drop the formula down the entire column to strip out unnecessary spaces, converting multiple spaces into single spaces

### 4. Identifying and Deleting Blank Rows
To find empty cells and remove their respective rows in one go:

* Highlight the column you want to check for blanks (e.g., Column D).
* On the ribbon, go to **Find & Select** and choose **Go To Special**.
* Select the **Blanks** option. This will automatically highlight all empty cells within your selected column.
* Right-click on any of the highlighted blank cells, choose **Delete**, and then select **Entire row**. This deletes all rows associated with those blank values simultaneously.

### 5. Removing Duplicates
To extract unique values and strip out duplicate entries:
* Select the column you want to filter, such as the Country column. 
* Navigate to the **Data** tab on the Excel ribbon.
* Click on the **Remove Duplicates** function. Excel will process the column, instantly remove the duplicate values, and leave only the unique entries behind.

# Data Transformation in Excel

This guide outlines key data transformation and analysis techniques in Microsoft Excel, focusing on calculating ratios and utilizing Pivot Tables to aggregate data, identify outliers, and find duplicates.

## 1. Calculating Ratios and New Metrics
You can create new data points by computing ratios between existing columns.
* Identify the columns you want to compare (e.g., Metro Area in column G and City Area in column D).
* In a new column, enter a simple division formula, such as `=G2/D2`.
* To apply this formula to the entire dataset, simply double-click the bottom-right corner of the cell (the fill handle) to autofill the values down the column. 
* This technique can be used to generate comparative metrics, such as population density or crowding, by comparing metro and city population ratios.

## 2. Inserting a Pivot Table
Pivot Tables provide aggregated information from your records, helping you understand large datasets at a glance.
* Select your entire dataset.
* Go to the **Insert** tab on the ribbon and click **Pivot Table**.
* Choose to create the Pivot Table in a **New Worksheet** and click OK.

## 3. Identifying Duplicates and Record Counts
You can use Pivot Tables to quickly see how many times a specific entry appears in your dataset.
* Drag a category, such as "Country", into the **Rows** section of the Pivot Table field list to see a unique list of entries.
* Drag that exact same "Country" field into the **Values** section. Excel will default to counting the entries.
* This allows you to easily identify if a value appears only once or multiple times (duplicates), providing a quick summary of your data's frequency.

## 4. Aggregating Data for Insights
Pivot Tables allow you to summarize numerical data across different categories.
* Drag categorical fields (like Country and City) into the **Rows** section.
* Drag a numerical field (like Population) into the **Values** section.
* You can change the value field settings to show the **Sum**, **Count**, or other aggregations to get a clear, hierarchical view of your dataset (e.g., viewing total population per country and breaking it down per city).

## 5. Using Pivot Charts to Identify Outliers
Visualizing your aggregated Pivot Table data helps in identifying trends and outliers.
* With your Pivot Table selected, insert a chart, such as a **Bar Chart**.
* Use the built-in filtering options on the chart or Pivot Table to narrow down your view (e.g., filtering to only show data for a specific country like the US or China).
* Analyze the visual distribution to spot outliers—such as cities with exceptionally high populations or density ratios compared to the rest of the group.

# Text to Columns in Excel

The **Text to Columns** utility in Excel is a powerful feature found under the **Data** tab. It allows you to split a single column of text into multiple columns based on specific delimiters (characters that separate the data). This is especially useful when cleaning up data scraped from websites or converting messy text into a structured format like a CSV file.

## 1. Accessing the Tool
Before using the tool, ensure your data is pasted into a single column. 
* Highlight the column containing the data you want to split.
* Navigate to the **Data** tab on the Excel ribbon.
* Click on **Text to Columns** located near the top.

## 2. Choosing the Delimited Option
When the Text to Columns wizard opens, you will be presented with two choices: Delimited or Fixed width.
* Select **Delimited**, as this allows you to split the text based on specific characters (like commas, hyphens, or parentheses) rather than strict spacing.
* Click **Next**.

## 3. Splitting by Custom Delimiters
If your data contains custom characters separating the values (such as a left parenthesis before a political party name), you can split the text iteratively.
* In the delimiters section, uncheck default options like "Tab" and check the box for **Other**.
* Type your specific custom delimiter into the box next to "Other" (for example, type a left parenthesis `(`).
* The data preview will show the text split into two columns. Click **Next** and then **Finish**. 
* The first part of your data (e.g., the Senator's name) is now isolated in its own column.

## 4. Iterating the Process
You will often need to repeat the Text to Columns process on the remaining unsplit data to isolate further variables.
* Select the newly created column that still contains multiple pieces of combined data.
* Open **Text to Columns** again, choose **Delimited**, and go to **Next**.
* Use the **Other** option to split by the next character in your sequence (e.g., using a hyphen `-` to separate a party from a state).
* Repeat this step using closing parentheses `)` or other custom characters until all nested data is separated.

## 5. Using Standard Delimiters
Excel also has built-in options for common delimiters.
* If your remaining data is separated by a comma (e.g., separating a state from a "yea" or "nay" vote), select the column and open the wizard.
* Instead of using "Other", simply check the **Comma** box in the delimiter list.
* Click **Next** and **Finish**.

## 6. Cleaning Up
Once all the text is successfully split into individual columns:
* Delete any extra or blank columns that were generated during the splitting process.
* Insert a new row at the top of your dataset to create clear headers for your newly separated data (e.g., "Senator", "Party", "State", "Vote").
* Apply formatting to make the data neat and ready for analysis.


# Data Aggregation in Excel

Data aggregation allows you to summarize large datasets, helping you examine trends, make comparisons, and reveal insights that might not be observable when viewing raw data in isolation. This guide covers how to prepare your data, use Pivot Tables to aggregate it, and apply visual features like color scales, sparklines, and data bars.

## 1. Data Cleanup
Before aggregating data, you must remove missing or empty values to ensure accurate analysis.
* Delete any completely empty columns that are not needed for your analysis.
* To remove rows containing blank cells in a specific column, select the entire column.
* Go to **Find & Select** on the ribbon and choose **Go To Special**.
* Select the **Blanks** option and click OK to highlight all empty cells.
* Right-click on a highlighted cell, choose **Delete**, and select **Entire row**.

## 2. Data Manipulation and Feature Extraction
To aggregate data by time periods (like weeks, months, or years), you may need to extract these details from a standard Date column.
* First, convert your data range into an Excel Table by navigating to **Insert** > **Table**. Tables automatically apply formulas down entire columns.
* Create new columns for Week, Month, and Year.
* **Week:** Use the formula `=WEEKNUM(date_cell, 1)` to extract the week number (starting on Sunday). Format the column as a Number with no decimal places.
* **Month:** Use the formula `=TEXT(date_cell, "mmm")` to extract the abbreviated month name.
* **Year:** Use the formula `=TEXT(date_cell, "yyyy")` to extract the four-digit year.

## 3. Visualizing Clusters with Color Scales
Color scales help you quickly identify high and low clusters within a column of numbers.
* Select the column containing your numerical data (e.g., New Cases).
* Go to **Conditional Formatting** > **Color Scales** > **More Rules**.
* Choose a **3-color scale**. You can adjust the colors so that low numbers are green (indicating a good or low state) and high numbers are red.
* Zooming out of the spreadsheet will allow you to easily spot overall clusters and trends visually.

## 4. Aggregating Data with Pivot Tables
Pivot Tables are the primary tool for aggregating data across various categories.
* Select your entire table (shortcut: `Ctrl + Shift + Down Arrow`, then `Right Arrow`).
* Go to **Insert** > **Pivot Table** and place it in your desired worksheet.
* **To aggregate by week:** Drag your "Location" (or country) field into the **Rows** section, your "Week" field into the **Columns** section, and your numerical data (e.g., "New Cases") into the **Values** section. 
* Excel will default to the **Sum** of your values, though you can change this to Average, Max, etc., under Value Field Settings.
* Drag the "Year" field above the "Location" or "Week" field to segment your aggregated data by year.

## 5. Identifying Trends with Sparklines
Sparklines are miniature charts placed inside single cells that help you quickly spot trends over time without looking at raw Pivot Table numbers.
* Navigate to the **Insert** tab and select **Line** under the Sparklines group.
* Select the data range across your time period (e.g., weekly data for a specific year) and choose where to place the sparkline.
* Under the Sparkline tab on the ribbon, you can enhance the visual by checking **High Point** and **Low Point** to add markers to the peaks and valleys of your trend line. 

## 6. Graphic Illustration using Data Bars
Data Bars provide an in-cell bar chart, perfect for viewing the rise and fall of numbers, such as a "wave" of cases across months.
* Create a Pivot Table summarizing data by Month (Rows) and Location (Columns).
* Select the column of aggregated numbers for a specific location.
* Go to **Conditional Formatting** > **Data Bars** and choose either a Gradient Fill or Solid Fill.
* The length of the data bar represents the value in the cell, making it easy to see when numbers reached their peak and when they declined.