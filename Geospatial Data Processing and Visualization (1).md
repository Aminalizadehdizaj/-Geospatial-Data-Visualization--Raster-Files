# Geospatial Data Processing and Visualization

## Raster files (Digital Elevation Models plot)
**_This Python script automates the generation of elevation maps from raster files in a specified folder. It utilizes GDAL for raster file handling and Matplotlib for visualization._**  


```python
from osgeo import gdal
import matplotlib.pyplot as plt
import numpy as np
import os

# Folder containing raster files
folder_path = r'G:\python\rasters_folder'
output_folder = rf'G:\output rasters'  # Folder to save PNG files

# Function to plot elevation and save as PNG
def plot_and_save_elevation(elevation, extent, filename):
    plt.figure(figsize=(8, 6))
    plt.imshow(elevation, cmap='terrain', extent=extent)
    plt.colorbar(label='Elevation')
    plt.title(f'Elevation Map: {filename}')  # Add filename as title
    plt.xlabel('X Coordinate')
    plt.ylabel('Y Coordinate')
    plt.grid(True)
    
    plt.gca().get_yaxis().set_major_formatter(plt.FuncFormatter(lambda x, loc: "{:.0f}".format(x)))  # Format y-axis ticks as integers
    
    # Add space around the plotted map from the axes (x and y axes)
    x_min, x_max, y_min, y_max = extent
    x_range = x_max - x_min
    y_range = y_max - y_min
    
    x_buffer = 0.02 * x_range  # 2% of x-axis interval
    y_buffer = 0.02 * y_range  # 2% of y-axis interval
    
    plt.xlim(x_min - x_buffer, x_max + x_buffer)
    plt.ylim(y_min - y_buffer, y_max + y_buffer)
    
    # Save as PNG
    plt.savefig(os.path.join(output_folder, f'{filename[:-4]}.png'), bbox_inches='tight')
    plt.close()  # Close the plot to avoid displaying it
    
    # Display plot
    plt.imshow(elevation, cmap='terrain', extent=extent)
    plt.colorbar(label='Elevation')
    plt.title(f'Elevation Map: {filename}')  # Add filename as title
    plt.xlabel('X Coordinate')
    plt.ylabel('Y Coordinate')
    plt.grid(True)
    
    # Add space around the plotted map from the axes (x and y axes)
    plt.xlim(x_min - x_buffer, x_max + x_buffer)
    plt.ylim(y_min - y_buffer, y_max + y_buffer)
    
    plt.gca().get_yaxis().set_major_formatter(plt.FuncFormatter(lambda x, loc: "{:.0f}".format(x)))  # Format y-axis ticks as integers
    plt.show()

# Loop through each file in the folder
for filename in os.listdir(folder_path):
    if filename.endswith('.tif'):  # Assuming raster files are in TIFF format
        file_path = os.path.join(folder_path, filename)
        
        # Open the raster file
        dataset = gdal.Open(file_path)
        if dataset is None:
            print(f'Could not open {filename}')
            continue
        
        # Read the raster data as a numpy array
        band = dataset.GetRasterBand(1)
        elevation = band.ReadAsArray()
        
        # Get geospatial information to calculate extent
        transform = dataset.GetGeoTransform()
        x_origin = transform[0]
        y_origin = transform[3]
        pixel_width = transform[1]
        pixel_height = transform[5]
        cols = dataset.RasterXSize
        rows = dataset.RasterYSize
        
        # Calculate extent
        x_min = x_origin
        x_max = x_origin + cols * pixel_width
        y_max = y_origin
        y_min = y_origin + rows * pixel_height
        
        extent = [x_min, x_max, y_min, y_max]
        
        # Mask NoData values if applicable
        no_data_value = band.GetNoDataValue()
        if no_data_value is not None:
            elevation = np.ma.masked_equal(elevation, no_data_value)
        
        # Plot the elevation map with extent and save as PNG
        plot_and_save_elevation(elevation, extent, filename)
```
