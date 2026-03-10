# Model the Data: Correlation with Excel
## Step 1: Enable the Data Analysis ToolPak
Before beginning the analysis, ensure the Excel Data Analysis ToolPak is enabled:
1. Go to **File** > **Options**.
2. Select **Add-ins** from the left-hand menu.
3. At the bottom of the window, ensure **Manage** is set to **Excel Add-ins** and click **Go**.
4. Check the box for **Analysis ToolPak** and click **OK**.
5. You can now access the tool in the **Data** tab of the ribbon, under **Data Analysis** (located near the Solver tool).

## Step 2: Generate a Correlation Matrix
1. Prepare your data by isolating the three target columns (New cases, New deaths, and New vaccinations) and deleting any unnecessary columns.
2. Go to the **Data** tab and click **Data Analysis**.
3. Select **Correlation** from the options list and click **OK**.
4. **Input Range**: Select the data for the three variables. Check the box for "Labels in first row".
5. **Output Range**: Choose to place the output in the same worksheet. Select an arbitrary range of rows and columns to hold the matrix, then click **OK**.

### Interpreting the Matrix Results
Correlation values range from -1 (absolute negative correlation) to +1 (absolute positive correlation):
* **New Cases vs. New Deaths**: Shows a correlation coefficient of **0.84**. This represents a very strong positive correlation.
* **New Vaccinations vs. New Deaths**: Shows a correlation coefficient of **0.23**. This is above 0, meaning it is a positive correlation, but it is very weak compared to cases vs. deaths.

## Step 3: Visualize Data with Scatterplots
To better understand the correlation, you can plot the values on a scatterplot.

### Plot 1: New Cases vs. New Deaths
1. Select the data for "New cases" (x-axis) and "New deaths" (y-axis).
2. Go to **Insert** > **Recommended Charts** > **All Charts** and select **X Y (Scatter)**. Choose the appropriate plot and click **OK**.
3. Add a trendline by clicking **Chart Elements** > **Trendline**.
4. Format the trendline to make it thicker and change the dash type to solid and the color to red.
* **Insight**: The trendline has a steep slope, visually confirming the strong positive correlation (0.84) shown in the matrix.

### Plot 2: New Vaccinations vs. New Deaths
1. Arrange your data so "New vaccinations" is on the x-axis and "New deaths" is on the y-axis (you may need to copy and paste the columns next to each other).
2. Select the columns and go to **Insert** > **Recommended Charts** > **All Charts** > **X Y (Scatter)**. Click **OK**.
3. Add a trendline, make it thicker, solid, and red.
* **Insight**: The slope of this trendline is much less steep, reflecting the weaker positive correlation (0.23). The plot shows that initially, when vaccinations were low, deaths were high. As the number of vaccinations increased, a decrease in the number of new deaths could be observed.


# Model the Data: Regression with Excel

**Variables in this Analysis:**
*   **Dependent Variable (Y):** New deaths
*   **Independent Variables (X):** New cases, new tests, new vaccinations, and stringency index.

## Step 1: Data Preparation
Before running the regression, the data must be prepared for easier interpretation. 
1. Create three new columns: **New cases per 1000**, **New tests per 1000**, and **New vaccinations per 1000**.
2. Populate these columns by dividing the original values of new cases, new tests, and new vaccinations by 1000. 

## Step 2: Running the Regression Tool
Ensure the Excel Data Analysis ToolPak is enabled (as covered in the correlation tutorial).
1. Go to the **Data** tab on the ribbon and click **Data Analysis**.
2. Scroll down, select **Regression**, and click **OK**.
3. **Input Y Range:** Select the data column for your dependent variable (`New deaths`).
4. **Input X Range:** Select the columns for your independent variables (`New cases per 1000`, `New tests per 1000`, `New vaccinations per 1000`, and `Stringency index`).
5. Ensure the **Labels** box is checked if your selected ranges include the column headers.
6. Under Output options, choose **New Worksheet Ply** and click **OK**.

## Step 3: Interpreting the Output
Once Excel generates the summary output, evaluate the model's validity and understand the relationships by checking the following metrics:

### 1. Adjusted R Square
For multiple linear regression, rely on the **Adjusted R Square** rather than the standard R Square.
*   **Result:** The value is **0.816**.
*   **Meaning:** 81.6% of the variation in the number of new deaths (Y) can be explained by the four independent variables in this model.

### 2. Significance F (Overall Model Validity)
Look at the ANOVA table for the `Significance F` value.
*   **Result:** The value is well below 0.05.
*   **Meaning:** The overall multiple linear regression model is a good fit and statistically significant.

### 3. P-values (Variable Significance)
Check the P-value for each independent variable to see if they are statistically significant (value < 0.05).
*   **New cases, New tests, New vaccinations:** P-values are below 0.05. They are significant and should be included in the model.
*   **Stringency index:** P-value is 0.11 (greater than 0.05). This variable is not statistically significant and therefore cannot be reliably included in the model.

### 4. Coefficients (Understanding the Relationships)
The mathematical model relies on coefficients to explain how each variable impacts the dependent variable, assuming all other variables are held constant:
*   **New cases per 1000:** Coefficient is **~7**. For every 1,000 new cases, the number of deaths increases by 7.
*   **New tests per 1000:** Coefficient is **0.69**. For every 1,000 new tests, the number of deaths increases by 0.69.
*   **New vaccinations per 1000:** Coefficient is **-0.07**. This negative value suggests that for every 1,000 new vaccinations, deaths decrease by 0.07. However, because this number is so small, you cannot use it to make a conclusive judgment without further analysis.
*   **Stringency index:** Coefficient is **2.71**. This implies that as policies become more restrictive, deaths actually increase. While this seems counterintuitive (stringent policies should decrease deaths), the P-value for stringency is > 0.05, meaning this finding is not statistically valid to draw conclusions from, but rather points to an area requiring deeper data analysis.