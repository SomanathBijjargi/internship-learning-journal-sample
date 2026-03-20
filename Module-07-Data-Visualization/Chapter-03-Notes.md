# RAWGraphs Visualization Reference Guide

This guide covers the setup and data mapping requirements for 10 advanced chart types available in [RAWGraphs](https://app.rawgraphs.io/).

---

## 1. Flow & Hierarchy Diagrams

### Alluvial & Sankey Diagrams
*Used to show flows, correlations, or changes in state.*
*   **Data Structure:** Multiple columns of categories (dimensions) and one column of values (measure).
*   **Mapping:**
    *   **Steps:** Drag 2 or more categorical columns here in the desired order.
    *   **Size:** Drag your numerical value (count or weight).
*   **Difference:** Alluvials are better for categorical changes; Sankeys are better for energy/cost flow.

### Circle Packing & Treemaps
*Used to show hierarchical relationships and parts-to-whole proportions.*
*   **Data Structure:** A nested hierarchy (e.g., Region > Country > City) and a numerical value.
*   **Mapping:**
    *   **Hierarchy:** Drag categories from broadest to most specific.
    *   **Size:** Drag your numerical value.
    *   **Color:** Map to a top-level category for better grouping.

### Sunburst Diagram
*A radial treemap showing hierarchy through concentric circles.*
*   **Mapping:** Identical to Treemaps.
*   **Tip:** Great for "drilling down" into data, though outer layers become harder to read if they are too thin.

---

## 2. Distribution & Ranking

### Beeswarm Diagram
*Shows individual data points to reveal density without overlapping.*
*   **Data Structure:** One numerical column and one categorical column.
*   **Mapping:**
    *   **X-Axis:** Your primary numerical value.
    *   **Group:** Your categorical column (creates horizontal "lanes").
    *   **Color:** Optional categorical mapping.

### Bump Chart
*Tracks changes in rank (position) over a sequence or time.*
*   **Data Structure:** Time/Sequence, Entity Name, and a Value that determines rank.
*   **Mapping:**
    *   **X-Axis:** Date or Sequence.
    *   **Streams:** The entities/names being ranked.
    *   **Size:** The value that RAWGraphs will use to calculate the ranking.

---

## 3. Over-Time & Spatial Trends

### Streamgraph
*A flowing version of a stacked area chart.*
*   **Data Structure:** Time, Category, and Value.
*   **Mapping:**
    *   **Date:** Your time-based column.
    *   **Group:** Your categorical column.
    *   **Size:** Your numerical value.

### Hexagonal Binning
*Aggregates points into hexagons to show density in a scatter plot.*
*   **Data Structure:** Two numerical columns (X and Y coordinates).
*   **Mapping:**
    *   **X-Axis & Y-Axis:** Drag your two measures here.
    *   **Size:** Automatically calculated based on the number of points in each hex.

### Voronoi Diagram
*Partitions space based on the distance to specific points.*
*   **Mapping:**
    *   **X-Axis & Y-Axis:** Numerical coordinates.
    *   **Color:** Usually mapped to a category to distinguish the resulting "cells."

---

## Quick Mapping Summary Table

| Chart Type | Primary Focus | Main Mapping Fields |
| :--- | :--- | :--- |
| **Alluvial** | Flow/Change | Steps (Categories) + Size |
| **Beeswarm** | Density | X-Axis (Value) + Group |
| **Treemap** | Hierarchy | Hierarchy (Categories) + Size |
| **Bump Chart** | Ranking | Date + Streams + Size |
| **Sunburst** | Hierarchy | Hierarchy (Categories) + Size |
| **Hexbin** | Correlation | X-Axis + Y-Axis |

---

## General RAWGraphs Workflow
1. **Load Data:** Paste from Excel/Google Sheets or upload a CSV.
2. **Select Chart:** Choose the visual model.
3. **Map Variables:** Drag and drop your headers into the green mapping slots.
4. **Customize:** Adjust colors, widths, and margins in the left sidebar.
5. **Export:** Use **SVG** for further design work or **PNG** for quick sharing.

*_*_*_*___8-8-8-8-8-8-8-8-8-8-8-8-

# Seaborn Data Visualization Reference Guide

## 1. Environment & Styling
Before plotting, import the necessary libraries and set the visual theme.

```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# Set visual style and context
sns.set_style('whitegrid') # Options: white, darkgrid, whitegrid, dark, ticks
sns.set_context('paper', font_scale=1.4) # Options: paper, talk, poster
```

---

## 2. Distribution Plots
Used to visualize how numerical data is distributed across a range.

### Distribution Plot (`displot`)
*Shows a histogram and a Kernel Density Estimation (KDE) line.*
```python
sns.displot(df['column'], kde=True, bins=30)
```

### Joint Plot
*Compares two variables and shows their individual distributions on the axes.*
* **Kinds:** `scatter`, `reg` (regression), `resid` (residual), `kde`, `hex`.
```python
sns.jointplot(x='x_col', y='y_col', data=df, kind='hex')
```

### Pair Plot
*Creates a matrix of plots for every numerical column in a dataframe.*
```python
sns.pairplot(df, hue='categorical_column', palette='magma')
```

---

## 3. Categorical Plots
Used to visualize the relationship between a category and a numerical value.

### Bar & Count Plots
* **Bar Plot:** Shows the mean (or other estimator) of a variable.
* **Count Plot:** Shows the number of occurrences in a category.
```python
sns.barplot(x='category', y='value', data=df, estimator=np.std)
sns.countplot(x='category', data=df)
```

### Box & Violin Plots
* **Box Plot:** Visualizes quartiles, medians, and outliers.
* **Violin Plot:** Shows the distribution density (KDE) mirrored on both sides.
```python
sns.boxplot(x='day', y='total_bill', data=tips, hue='smoker')
sns.violinplot(x='day', y='total_bill', data=tips, hue='sex', split=True)
```

### Swarm & Strip Plots
* **Strip Plot:** Scatter plot where one axis is categorical.
* **Swarm Plot:** Like a strip plot, but points are adjusted so they do not overlap.
```python
sns.swarmplot(x='day', y='total_bill', data=tips, color='black')
```

---

## 4. Matrix Plots
Requires data to be in a matrix format (rows and columns represent variables).

### Heatmap
*Visualizes data through colors. Highly effective for correlation matrices.*
```python
# Create correlation matrix
corr_matrix = df.corr()
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm')
```

### Cluster Map
*Uses hierarchical clustering to group similar rows and columns.*
```python
sns.clustermap(pivoted_df, cmap='Blues', standard_scale=1)
```

---

## 5. Grid Systems
For creating complex multi-plot layouts.

### Pair Grid
*Full control over the diagonal, upper, and lower plots in a pair-wise grid.*
```python
g = sns.PairGrid(df)
g.map_diag(plt.hist)
g.map_upper(plt.scatter)
g.map_lower(sns.kdeplot)
```

### Facet Grid
*Splits data into a grid of subplots based on categorical values.*
```python
g = sns.FacetGrid(df, col='time', row='smoker')
g.map(plt.scatter, 'total_bill', 'tip')
```

---

## 6. Regression Plots
### LM Plot
*Plots a scatter plot and fits a linear regression model line.*
```python
sns.lmplot(x='total_bill', y='tip', data=tips, hue='sex', markers=['o', 'v'])
```

---

## General Tips
1. **Palettes:** Use `palette='viridis'`, `plasma`, `magma`, or `inferno` for modern color scales.
2. **Legend:** Reposition legends using `plt.legend(loc=0)` (0 is best, 1 is top-right).
3. **Save:** Use `plt.savefig('filename.png')` to export your charts.
