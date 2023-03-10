Create interactive maps, and discover patterns in geospatial data.
# 1.0 Your First Map
Get started with plotting in GeoPandas.
In this micro-course, you'll learn about different methods to wrangle and visualize geospatial data, or data with a geographic location.

<img src="https://user-images.githubusercontent.com/51888893/217288307-81b34ec3-3b70-4be2-80d3-a66d24540870.png" width=600px>

****Along the way, you'll offer solutions to several real-world problems like:****

- Where should a global non-profit expand its reach in remote areas of the Philippines?
- How do purple martins, a threatened bird species, travel between North and South America? Are the birds travelling to conservation areas?
- Which areas of Japan could potentially benefit from extra earthquake reinforcement?
- Which Starbucks stores in California are strong candidates for the next Starbucks Reserve Roastery location?
- Does New York City have sufficient hospitals to respond to motor vehicle collisions? Which areas of the city have gaps in coverage?
## 1.1 Reading data
```py
import geopandas as gpd
```
```py
# Read in the data
full_data = gpd.read_file("../input/geospatial-learn-course-data/DEC_lands/DEC_lands/DEC_lands.shp")

# View the first five rows of the data
full_data.head()
```
select only columns of interest
```py
data = full_data.loc[:, ["CLASS", "COUNTY", "geometry"]].copy()
```
```py
# How many lands of each type are there?
data.CLASS.value_counts()
```
```py
# Select lands that fall under the "WILD FOREST" or "WILDERNESS" category
wild_lands = data.loc[data.CLASS.isin(['WILD FOREST', 'WILDERNESS'])].copy()
wild_lands.head()
```
## 1.2 Create your first map!
```py
wild_lands.plot()
```
<img src="https://user-images.githubusercontent.com/51888893/217291124-884a3146-e735-43e8-854e-db9e933ea60f.png" width=100px>

****Every GeoDataFrame contains a special "geometry" column**** . It contains all of the geometric objects that are displayed when we call the plot() method.
```py
# View the first five entries in the "geometry" column
wild_lands.geometry.head()
```

            POLYGON ((486093.245 4635308.586, 486787.235 4...
            1    POLYGON ((491931.514 4637416.256, 491305.424 4...
            2    POLYGON ((486000.287 4635834.453, 485007.550 4...
            3    POLYGON ((541716.775 4675243.268, 541217.579 4...
            4    POLYGON ((583896.043 4909643.187, 583891.200 4...
While this column can contain a variety of different datatypes, each entry will typically be a ****Point, LineString, or Polygon.****
<img src="https://user-images.githubusercontent.com/51888893/217291656-43f90d10-507a-4709-a3e8-40fff76bc25c.png" width=900px>

```py
# Campsites in New York state (Point)
POI_data = gpd.read_file("../input/geospatial-learn-course-data/DEC_pointsinterest/DEC_pointsinterest/Decptsofinterest.shp")
campsites = POI_data.loc[POI_data.ASSET=='PRIMITIVE CAMPSITE'].copy()

# Foot trails in New York state (LineString)
roads_trails = gpd.read_file("../input/geospatial-learn-course-data/DEC_roadstrails/DEC_roadstrails/Decroadstrails.shp")
trails = roads_trails.loc[roads_trails.ASSET=='FOOT TRAIL'].copy()

# County boundaries in New York state (Polygon)
counties = gpd.read_file("../input/geospatial-learn-course-data/NY_county_boundaries/NY_county_boundaries/NY_county_boundaries.shp")
```
Next, we create a map from all four GeoDataFrames.
```py
# Define a base map with county boundaries
ax = counties.plot(figsize=(10,10), color='none', edgecolor='gainsboro', zorder=3)

# Add wild lands, campsites, and foot trails to the base map
wild_lands.plot(color='lightgreen', ax=ax)
campsites.plot(color='maroon', markersize=2, ax=ax)
trails.plot(color='black', markersize=1, ax=ax)
```
<img src="https://user-images.githubusercontent.com/51888893/217347790-364adb8c-80f9-4eb7-a65c-abf89c1d3149.png" width=600px>

## 1.3 Exercise: Your First Map
see code [here](https://github.com/ortizfram/Kaggle-Learning-Courses/blob/main/Geospatial%20Analysis/exercise-your-first-map.ipynb)
# 2.0 Coordinate Reference Systems
It's pretty amazing that we can represent the Earth's surface in 2 dimensions!
 the world is actually a three-dimensional globe. So we have to use a method called a ***map projection*** to render it as a flat surface.
 
- the equal-area projections (like "Lambert Cylindrical Equal Area", or "Africa Albers Equal Area Conic") preserve area. This is a good choice, if you'd like to calculate the area of a country or city, for example.
- the equidistant projections (like "Azimuthal Equidistant projection") preserve distance. This would be a good choice for calculating flight distance.
<img src="https://user-images.githubusercontent.com/51888893/217352930-8af47532-b3c6-475b-bbf2-cb6f5c822e5f.png" width=600px>

We use a coordinate reference system (CRS) to show how the projected points correspond to real locations on Earth
## 1.4 Setting the CRS
When we create a GeoDataFrame from a shapefile, the CRS is already imported for us.
```py
# Load a GeoDataFrame containing regions in Ghana
regions = gpd.read_file("../input/geospatial-learn-course-data/ghana/ghana/Regions/Map_of_Regions_in_Ghana.shp")
print(regions.crs)
```
            epsg:32630
This GeoDataFrame uses EPSG 32630, which is more commonly called the "Mercator" projection. This projection preserves angles (making it useful for sea navigation) and slightly distorts area.

However, when creating a GeoDataFrame from a CSV file, we have to set the CRS. EPSG 4326 corresponds to coordinates in latitude and longitude.
```py
# Create a DataFrame with health facilities in Ghana
facilities_df = pd.read_csv("../input/geospatial-learn-course-data/ghana/ghana/health_facilities.csv")

# Convert the DataFrame to a GeoDataFrame
facilities = gpd.GeoDataFrame(facilities_df, geometry=gpd.points_from_xy(facilities_df.Longitude, facilities_df.Latitude))

# Set the coordinate reference system (CRS) to EPSG 4326
facilities.crs = {'init': 'epsg:4326'}

# View the first five rows of the GeoDataFrame
facilities.head()
```
- We begin by creating a DataFrame containing columns with latitude and longitude coordinates.
- To convert it to a GeoDataFrame, we use gpd.GeoDataFrame().
- The gpd.points_from_xy() function creates Point objects from the latitude and longitude columns.
# 1.5 Exercise: Coordinate Reference Systems
see it [here]()
