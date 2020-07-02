```python
from netCDF4 import Dataset
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.basemap import Basemap

#######################################
#	Global variables
#
File_Of_Coord='tie_geo_coordinates.nc'
File_Of_Meteo='tie_meteo.nc'
HUM = 'humidity'
TOZON = 'total_ozone'
TCWV = 'total_columnar_water_vapour'
SEA_PRESSURE = 'sea_level_pressure'
#######################################

def extract_coords(coord_file):
        nc_c = coord_file                # Filename 'tie_geo_coordinates.nc'
        nc_coord = Dataset(nc_c, 'r')    # Dataset for coordinates
        lats = nc_coord.variables['latitude'][:] 
        lons = nc_coord.variables['longitude'][:]
        return lats, lons

def extract_param(param_file,_param):
        nc_m = param_file                # Filename 'tie_geo_meteo.nc' 
        nc_param = Dataset(nc_m, 'r')    # Dataset for meteo data
        param_to_plot = nc_param.variables[_param][:]
        return param_to_plot


root_grp = Dataset('satelite_sn.nc', 'w', format='NETCDF4')
root_grp.description = 'Satelite data for Senegal \n From https://peps.cnes.fr'

# dimensions
root_grp.createDimension('tie_row',4091)
root_grp.createDimension('tie_column',77)

# variables
latitudes = root_grp.createVariable('latitude', 'f4', ('tie_row', 'tie_column',))
longitudes = root_grp.createVariable('longitude', 'f4', ('tie_row', 'tie_column',))
total_ozone = root_grp.createVariable('total_ozone', 'f4', ('tie_row', 'tie_column',))
humidity = root_grp.createVariable('humidity', 'f4', ('tie_row', 'tie_column',))
total_columnar_water_vapour = root_grp.createVariable('total_columnar_water_vapour', 'f4', ('tie_row', 'tie_column',))
sea_level_pressure = root_grp.createVariable('sea_level_pressure', 'f4', ('tie_row', 'tie_column',))

# data
latitudes[:],longitudes[:] = extract_coords(File_Of_Coord)
total_ozone[:] = extract_param(File_Of_Meteo,TOZON)
humidity[:] = extract_param(File_Of_Meteo,HUM)
total_columnar_water_vapour[:] = extract_param(File_Of_Meteo,TCWV)
sea_level_pressure[:] = extract_param(File_Of_Meteo,SEA_PRESSURE)

# group
# my_grp = root_grp.createGroup('my_group')

root_grp.close()
```
