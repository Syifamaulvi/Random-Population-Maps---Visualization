# Random-Population-Maps-Visualization
In this section, random population location points will be generated using Python. This visualization is carried out to observe the distribution of population points within a certain area, either with proportional comparisons or by using actual data.

## Concept
Dot map: each point represents one (or a few) individuals/events. Great for showing where things happen and detecting spatial patterns or gaps.

Data you need:
A boundary polygon of the area in GeoJSON/GeoPackage (Theres a Bandung Maps via GeoJSON)
A point table with coordinates (Longitude, Latitude) in WGS84 (EPSG:4326).

## Environment & files
pip install geopandas shapely matplotlib pandas

## Load data & align coordinate reference systems (CRS)
Make sure the boundary and points use the same CRS (WGS84/EPSG:4326).

import geopandas as gpd
import matplotlib.pyplot as plt
import pandas as pd

### 1) Bandung boundary
bandung_boundary = gpd.read_file('Bandung_Boundary.geojson')

### 2) Points from CSV → GeoDataFrame
df_random_points = pd.read_csv('A_Populasi_2341.csv')  # columns: Longitude, Latitude
gdf_random_points = gpd.GeoDataFrame(
    df_random_points,
    geometry=gpd.points_from_xy(df_random_points['Longitude'], df_random_points['Latitude']),
    crs='EPSG:4326'
)

Tips
Correct order is points_from_xy(Longitude, Latitude) → (x=lon, y=lat).
If your CSV is in UTM or another CRS, reproject to EPSG:4326 before plotting.

## (Recommended) Keep only points inside Bandung
bandung_poly = bandung_boundary.unary_union  # merge polygons if multiple
gdf_random_points = gdf_random_points[gdf_random_points.geometry.within(bandung_poly)]

## Plot a clean dot map & save a high-res image
fig, ax = plt.subplots(figsize=(10, 10))

bandung_boundary.plot(ax=ax, color='lightgrey', edgecolor='white', linewidth=0.6)
gdf_random_points.plot(ax=ax, markersize=2, color='black', label='Population Points')

ax.legend()
ax.set_title('Dispersion of Infants in Bandung City', pad=12)
ax.set_xlabel('Longitude'); ax.set_ylabel('Latitude')
ax.set_aspect('equal')

plt.tight_layout()
fig.savefig('Dispersion_of_Infants_Bandung.png', dpi=300, bbox_inches='tight')
plt.show()

<img width="682" height="456" alt="image" src="https://github.com/user-attachments/assets/bf46942c-bd13-4abd-91e6-52e71e7fa840" />

you can use the file to practice!


