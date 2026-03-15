
# Geospatial Analysis with Python: Comprehensive Guide

## 1. Environment Setup
To begin, you need to install and import the core libraries.
* **Pandas**: Data manipulation.
* **Geopy**: Geocoding and distance calculations.
* **Folium**: Interactive map generation.
* **Geopandas**: Handling geospatial vector data (Shapefiles).

```python
import pandas as pd
import numpy as np
import folium
import geopy.distance
from geopy.geocoders import Nominatim
import geopandas

```

## 2. Distance Calculations

The `geopy` library allows you to calculate the geodesic distance between two points on the Earth's surface.

### A. Geocoding Locations

Convert address strings into latitude and longitude coordinates:

```python
geolocator = Nominatim(user_agent="geo_analysis")
loc1 = geolocator.geocode("Moradabad")
loc2 = geolocator.geocode("Dehradun")

coords_1 = (loc1.latitude, loc1.longitude)
coords_2 = (loc2.latitude, loc2.longitude)

```

### B. Calculating Distance

```python
# Calculate distance in Kilometers
dist = geopy.distance.distance(coords_1, coords_2).km
print(f"Distance between locations: {dist} km")

```

## 3. Working with Datasets

When dealing with multiple locations (e.g., store locations), we use Pandas to calculate distances relative to a central point.

```python
# Define a central reference point (e.g., Empire State Building)
center_point = (40.748488, -73.985238)

# Calculate distance for every row in a dataframe
def calculate_dist(row):
    store_coords = (row['lat'], row['lng'])
    return geopy.distance.distance(center_point, store_coords).km

df['distance_to_center'] = df.apply(calculate_dist, axis=1)

```

## 4. Interactive Data Visualization

Folium is used to create web-based maps. You can add markers and heatmaps to visualize the density of your data.

### A. Initializing the Map

```python
# Create a base map centered on a specific coordinate
m = folium.Map(location=[40.748488, -73.985238], zoom_start=12)

```

### B. Adding Markers and Popups

```python
for i, row in df.iterrows():
    folium.Marker(
        location=[row['lat'], row['lng']],
        popup=f"Store: {row['store_name']}",
        tooltip="Click for info"
    ).add_to(m)

# To view the map in a notebook
m

```

## 5. Advanced Analysis with GeoPandas

For spatial joining and working with boundaries (like borough or city shapes), use GeoPandas.

```python
# Convert a standard DataFrame to a GeoDataFrame
geom = geopandas.points_from_xy(df.lng, df.lat)
gdf = geopandas.GeoDataFrame(df, geometry=geom)

# Exporting to a Shapefile for use in GIS software
gdf.to_file('output_map/points.shp')

```

## Summary of Workflow

1. **Extract**: Load location data from Excel or CSV.
2. **Transform**: Use `geopy` to find coordinates and calculate distances.
3. **Analyze**: Filter data based on proximity or spatial boundaries.
4. **Visualize**: Plot results on an interactive `folium` map or export via `geopandas`.

---

# Geospatial Analysis: Creating Shapefiles with QGIS

## 1. Introduction to QGIS

QGIS is essential for storing geographic information (latitude, longitude, and demographics) for countries, states, and cities. While many shapefiles are available for free online, QGIS allows you to create custom ones when specific data is missing.

### Installation

* **Website**: [qgis.org](https://qgis.org)
* **Versions**: Available for Windows (32-bit and 64-bit), macOS, and Linux.
* **Recommended**: The standalone installer (e.g., version 3.16) is often sufficient for most geospatial tasks.

---

## 2. Navigating the Interface

* **Toolbars**: Located at the top. The **Digitizing Toolbar** is critical for creating new features.
* **Browser Panel**: Used to navigate your computer's files.
* **Layers Panel**: Displays all active maps and data layers in your project.
* **Panels Management**: Access missing panels via `View > Panels`.

---

## 3. Working with Existing Shapefiles

Free spatial data can be sourced from sites like **Diva-GIS**.

### Importing Data

1. Download and extract the zip file containing the shapefiles.
2. Drag and drop the `.shp` files into the QGIS Layers panel.
3. **Administrative Levels**: Shapefiles often come in layers (e.g., admin 0 for country, admin 1 for states, etc.).

### Analyzing Attributes

* **Attribute Table**: Right-click a layer and select `Open Attribute Table` to see data like names, IDs, and demographics.
* **Selection**: Selecting a row in the table highlights the corresponding area on the map.

### Customizing Appearance (Symbology & Labels)

* **Color**: Right-click layer > `Properties > Symbology` to change map colors.
* **Labels**: Right-click layer > `Properties > Labels > Single Labels`. Select a field (e.g., "Name 3") to display administrative names directly on the map.

---

## 4. Using Web Map Plugins

To see how your shapefile fits in the real world, use the **QuickMapServices** plugin.

1. Go to `Plugins > Manage and Install Plugins`.
2. Search for "Quick Map Services" and install.
3. Go to `Web > Quick Map Services > OSM > OSM Standard` to overlay your data on an OpenStreetMap base map.

---

## 5. Creating a Custom Shapefile (South Sudan Example)

When a country or region is missing from standard datasets, you can manually digitize it.

### Step-by-Step Creation

1. **New Layer**: Go to `Layer > Create Layer > New Shapefile Layer`.
2. **Configuration**:
* **Geometry Type**: Select **Polygon**.
* **Attributes**: Add fields like "Name" (Text type).


3. **Digitizing**:
* Select the layer and click **Toggle Editing** (pencil icon).
* Select **Add Polygon Feature**.
* Left-click along the boundaries of the region on the base map.
* Right-click to finish the polygon and enter attribute data (e.g., ID: 1, Name: South Sudan).



---

## 6. Exporting Your Work

Once created, you can export your data for use in other applications.

* **Shapefile (.shp)**: Standard format for GIS software.
* **KML File**: Ideal for viewing in **Google Earth**.
* **Process**: Right-click layer > `Export > Save Features As`. Choose your format and save location.

---

## 7. Verification in Google Earth

Double-clicking a saved KML file will open it in Google Earth, where the custom polygon you drew in QGIS will be overlaid precisely on the 3D globe, complete with the attributes you assigned.