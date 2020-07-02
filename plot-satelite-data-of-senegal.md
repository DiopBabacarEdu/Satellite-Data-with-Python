```python
import numpy as np
from netCDF4 import Dataset  # http://code.google.com/p/netcdf4-python/
import matplotlib.pyplot as plt
from mpl_toolkits.basemap import Basemap

HUM = 'humidity'
TOZON = 'total_ozone'
TCWV = 'total_columnar_water_vapour'
SEA_PRESSURE = 'sea_level_pressure'

def plot_parameter(coord_file,param_file,_param):
	nc_c = coord_file  # Filename 1
	nc_m = param_file            # Filename 2
	nc_coord = Dataset(nc_c, 'r')    # Dataset for coordinates
	nc_param = Dataset(nc_m, 'r')    # Dataset for meteo data

	lats = nc_coord.variables['latitude'][:]  # extract/copy the data
	lons = nc_coord.variables['longitude'][:]
	param_to_plot = nc_param.variables[_param][:]
	print(lats)
	
	
	map = Basemap(llcrnrlon=-18.,llcrnrlat=11.9,urcrnrlon=-10.,urcrnrlat=17.,resolution='i', projection='tmerc', lat_0 = 14.666020, lon_0 = -14.787668)
	#map = Basemap(llcrnrlon=-18.,llcrnrlat=11.9,urcrnrlon=-10.,urcrnrlat=17.,resolution='i', projection='tmerc', lat_0 = 14.666020, lon_0 = -14.787668)
	#map=Basemap(projection='mill',lat_ts=10,llcrnrlon=lons.min(),urcrnrlon=lons.max(),llcrnrlat=lats.min(),urcrnrlat=lats.max(),resolution='c')
	map.drawcoastlines()
	map.drawstates()
	map.drawcountries()
	map.drawlsmask(land_color='Linen', ocean_color='aqua')

	#lon, lat = np.meshgrid(lons, lats)
	x, y = map(lons, lats)

	param_ = map.contourf(x,y,param_to_plot[:])
	cb = map.colorbar(param_,"bottom", size="5%", pad="2%")
	plt.title(nc_param.variables[_param].long_name)
	cb.set_label("%s (%s)" % (nc_param.variables[_param].long_name,nc_param.variables[_param].units))
	#plt.show()

plot_parameter('tie_geo_coordinates.nc','tie_meteo.nc',TOZON)
#plot_parameter('tie_geo_coordinates.nc','tie_meteo.nc',HUM)
#plot_parameter('tie_geo_coordinates.nc','tie_meteo.nc',TCWV)
#plot_parameter('tie_geo_coordinates.nc','tie_meteo.nc',SEA_PRESSURE)
```
