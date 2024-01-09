# Raster Map Plotter
_** The project serves as both a tool for daily workflow enhancement and a personal development endeavor.**_  
_The script reads each TIFF file from the specified folder, extracts raster value data, calculates the geospatial extent of the raster, handles NoData values, and then generates a plot displaying the raster map. Finally, it saves the plot as a PNG file in an output folder._
## 
## Usage
### _1. Folder Structure:_
* Place the raster files (in TIFF format) in the folder specified by folder_path.  
* Ensure the output folder (output_folder) for saving PNG files is correctly set.
  
### _2. Dependencies:_
* Python 3.x  
* Required Python libraries: osgeo, matplotlib, numpy
  
### _3. Running the Script:_

* Modify folder_path to point to your raster files directory.  
* Adjust output_folder to specify the folder where PNG files will be saved.
* Run the script.
* The script will iterate through each TIFF file, plot the raster map, and save it as a PNG file in the output folder.
## Example
- rasters_DEM/
  - elevation_map_1.tif
  - elevation_map_2.tif
  - ...
- output_DEM/
  - elevation_map_1.png
  - elevation_map_2.png
  - ...
  ## Achievements
  * Transformed daily workspace tasks by automating the handling of numerous raster files, replacing time-consuming manual methods in ArcGIS/QGIS.
  * Implemented matplotlib to create visual representations of elevation maps and saved them as PNG files, significantly reducing processing time and enhancing efficiency.
 
  
