# Solar Site Assessment, Allegheny County PA
## DAT241 Final Project


### ABSTRACT:

The advent of renewable energy makes finding possible locations for wind and solar farms/facilities an important step in the shift of energy sources.  Spatial software makes the task easier, but involves multiple data sources and process steps.  An initial highgrading of locations can involve basic spatial and geographic values, such as slope severity, slope aspect, and distance to existing infrastructure.  Utilizing these parameters, the number of acres falling with Allegheny County, Pennsylvania municipal areas was calculated for this project, resulting in a final map and table highgrading focus areas for further analysis.


### INQUIRY:

In this project, ArcGIS Pro was used to analyse attributes of Allegheny County, PA to determine prospective locations for solar facilities.  It focuses on slope (less than 20 degrees), distance from a transmission line (within one mile), slope aspect (south facing between 135-225 degrees), and current land cover/usage (forest/grasslands, agricultural, and strip mine) then ranks municipal areas based on the number of acres of potential property.  


### DATA SOURCES:

states - https://www.arcgis.com/home/item.html?id=540003aa59b047d7a1f465f7b1df1950

Allegheny County - https://data.wprdc.org/dataset/allegheny-county-boundary1

parcels - https://data.wprdc.org/dataset/allegheny-county-parcel-boundaries1

transmission lines - https://hifld-geoplatform.opendata.arcgis.com/datasets/electric-power-transmission-lines/data?geometry=-90.359%2C38.687%2C-73.011%2C41.625

municipal boundaries - https://openac-alcogis.opendata.arcgis.com/datasets/allegheny-county-municipal-boundaries?geometry=-80.887%2C40.251%2C-79.153%2C40.617

US elevation data - https://elevation.arcgis.com/arcgis/services/NED30m/ImageServer

land cover & land use - https://data.wprdc.org/dataset


### METHODOLOGY:

Spatial Selection - narrow down state outlines to WV/PA/OH (tristate)

Clip Raster - clip elevation raster using Allegheny_County shapefile (Allegheny_Co_elev)

Clip Vector - clip transmission lines layer using tristate outline (transmission_lines_clip)

Buffer - create buffer around clipped transmission lines of 1 mile - reference https://www.ysgsolar.com/blog/solar-farm-land-requirements-solar-developments-ysg-solar#:~:text=As%20with%20power%20lines%2C%20proximity,risk%20associated%20with%20the%20project (trans_line_buffer)

Attribute Selection - narrow down land cover layer to just forest, grassland, agricultural, or strip mine (land_cover_potential)

Clip - clip land_cover_potential using trans_line_buff to find potential areas within one mile of a transmission line (land_near_transmission)

Slope - determine slope from Allegheny_Co_elev and highlight areas with less than 20 degrees - references https://www.esri.com/news/arcuser/1010/solarsiting.html#:~:text=Suitable%20slope%E2%80%94The%20slope%20should,of%20solar%20radiation%20per%20year and https://www.mdpi.com/1996-1073/9/8/643/pdf (AlleghenyCo_slope)

Aspect - create aspect raster from Allegheny_Co_elev highlighting south facing features of 135 to 225 degrees for optimal sun exposure (AlleghenyCo_aspect)

Polygon to Raster - convert land_near_transmission to a raster (land_raster)

Reclassify - reclassify AlleghenyCo_aspect, land_raster, and AlleghenyCo_slope to two class rasters - 1 for good, 0 for bad (aspect_reclass, land_reclass, and slope_reclass)

Raster Calculator - multiply the three reclassified rasters together to create a new raster that highlights land that has all three attributes (ideal_raster)

Raster to Polygon - convert ideal_raster to a polygon for clipping (ideal_polygon)

Explode - break ideal_polygon into multiple polygons

Summarize Within - tally the number of acres within each of the Municipal boundaries


### CONCLUSIONS:

It appears that proximity to infrastructure is the largest inflencer on which municipal areas contain the most prospective acreage.  The outermost areas rank higher, with Plum; North Fayette; and Findlay having all more than 2000 acres prospective.  Downsides are how contiguous the acreage is - for easier development larger blocks are ideal.  Further analysis could be to narrow down based on size.  Landowners could be determined using tract/parcel data for use in leasing and development plans.  There are likely additional spatial/geographic parameters that could be used to narrow down the site selection as well.

The one mile distance from transmission lines is based on a single resource, noted above, and was determined based on cost analysis to tie in. Financial metrics are likely to change depending on location and market, so the buffer size could be adjusted depending on the utility being tied into and prices for the area. 

The ideal slope ranged from 18 to 35 degrees based on the two references - the project was done using a 20 degree cutoff but could be adjusted depending on stability in the area or other tolerences that are likely to change depending on topography and geology.

It should also be noted that the elevation data does not take into account existing buildings, so there could be some acreage that is noted as prospective that will need to be thrown out due to presence of and proximity to structures.

##### MAP 1: intermediate layers to create final output

![solar_inputs](https://user-images.githubusercontent.com/71047291/117082846-4c3c4680-ad11-11eb-9d05-1c6599ec4132.jpg)



##### MAP 2: final output of the acreage that meets all the criteria noted above

![solar_final_results](https://user-images.githubusercontent.com/71047291/117085661-6168a380-ad18-11eb-9060-4461b0b7c837.jpg)


##### TABLE 1: table of acreage per municipal area

![table](https://user-images.githubusercontent.com/71047291/117084198-91ae4300-ad14-11eb-8ee5-5d8a62c16284.jpg)

