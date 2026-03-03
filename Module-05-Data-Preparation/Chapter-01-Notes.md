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
