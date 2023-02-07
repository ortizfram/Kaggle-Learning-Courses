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
While this column can contain a variety of different datatypes, each entry will typically be a ****Point, LineString, or Polygon.****
<img src="https://user-images.githubusercontent.com/51888893/217291656-43f90d10-507a-4709-a3e8-40fff76bc25c.png" width=900px>
