# The Eumetnet postprocessing benchmark dataset Climetlab plugin

A plugin for [climetlab](https://github.com/ecmwf/climetlab) for the Eumetnet postprocessing benchmark dataset.

Ease the download of the dataset time-aligned forecasts, reforecasts (hindcasts) and observations ([ERA5 reanalysis](https://www.ecmwf.int/en/forecasts/dataset/ecmwf-reanalysis-v5)).

## Using climetlab to access the data

See the [demo notebooks](https://github.com/Climdyn/climetlab-eumetnet-postprocessing-benchmark/tree/main/notebooks)
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Climdyn/climetlab-eumetnet-postprocessing-benchmark/main?urlpath=lab)


- [demo_training_data_forecasts.ipynb](https://github.com/Climdyn/climetlab-eumetnet-postprocessing-benchmark/tree/main/notebooks/demo_training_data_forecasts.ipynb)
  [![nbviewer](https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg)](https://nbviewer.jupyter.org/github/Climdyn/climetlab-eumetnet-postprocessing-benchmark/blob/main/notebooks/demo_training_data_forecasts.ipynb)
  [![Open in colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Climdyn/climetlab-eumetnet-postprocessing-benchmark/blob/main/notebooks/demo_training_data_forecasts.ipynb)
  [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Climdyn/climetlab-eumetnet-postprocessing-benchmark/main?filepath=notebooks/demo_training_data_forecasts.ipynb)
  [<img src="https://deepnote.com/buttons/launch-in-deepnote-small.svg">](https://deepnote.com/launch?name=MyProject&url=https://github.com/Climdyn/climetlab-eumetnet-postprocessing-benchmark/tree/main/notebooks/demo_my_dataset.ipynb)


The climetlab python package allows easy access to the data with a few lines of code such as:
``` python
# Uncomment the line below if climetlab and the plugin are not yet installed
#!pip install climetlab climetlab-eumetnet-postprocessing-benchmark
import climetlab as cml
ds = cml.load_dataset('eumetnet-postprocessing-benchmark-training-data-gridded-forecasts-surface', "2017-12-02", "2t", "highres")
fcs = ds.to_xarray()
```
which download the deterministic (high-resolution) forecasts for the 2 metre temperature. 
Once obtained, the corresponding observations can be retrieved in the [xarray](http://xarray.pydata.org/en/stable/index.html) format by using the `get_observations_as_xarray` method:
``` python
obs = ds.get_observations_as_xarray()
```

> **Remark:** For obvious reasons, observations are not available for Extreme Forecast Indices (EFI).

## Datasets description 

There are two main datasets: 

## 1 - Gridded Data


![](./docs/gridded_data.jpg)

 * The Eumetnet postprocessing benchmark dataset contains [ECMWF](https://www.ecmwf.int/) ensemble and deterministic forecasts over a large portion of Europe,  from 36 to 67° in latitude and from -6 to 17° of longitude,
and covers the years 2017-2018.

 * It also contains the corresponding ERA5 reanalysis for the purpose of providing observations for the benchmark.

 * For some dates, it contains also reforecasts that covers 20 years of past forecasts recomputed with the most recent model version.

 * All the forecasts and reforecasts provided are the noon ECMWF runs.

 * The ensemble forecasts and reforecsts also contain by default the control run.

 * The gridded data resolution is 0.25° x 0.25° which corresponds roughly to 25 kilometers. 

 * **Please note that you can presently only retrieve one forecast date** for each `climetlab.load_dataset` call.

There are 5 gridded sub-datasets:

### 1.1 - Extreme Forecast Index

All the [Extreme Forecast Index](https://www.ecmwf.int/assets/elearning/efi/efi1/story_html5.html) (EFI) variables can be obtained for each forecast date.

It includes:

| Parameter name                |  ECMWF key    |
|-------------------------------|---------------|
| 2 metre temperature efi     	|  2ti          |  
| 10 metre wind speed efi	    |  10ws         |
| 10 metre wind gust efi	    |  10fgi        |
| cape efi	                    |  capei        |
| cape shear efi	            |  capesi       |
| Maximum temperature at 2m efi	|  mx2ti        |
| Minimum temperature at 2m efi	|  mn2ti        |
| Snowfall efi	                |  sfi	        |
| Total precipitation efi	    |  tpi          |

The EFI are available for the model step range (in hours) 0-24, 24-48, 48-72, 72-96, 96-120, 120-144 and 144-168.

**Usage:** The EFI variables can retrieved by calling 

``` python
ds = cml.load_dataset('eumetnet-postprocessing-benchmark-training-data-gridded-forecasts-efi', date, parameter)
ds.to_xarray()
```

where the `date` argument is a string with a single date, and the `parameter` argument is a string or a list of string with the 
ECMWF keys described above. Setting `'all'` as `parameter` download all the EFI parameters.

**Example:**

``` python
ds = cml.load_dataset('eumetnet-postprocessing-benchmark-training-data-gridded-forecasts-efi', "2017-12-02", "2ti")
ds.to_xarray()
```

### 1.2 - Surface variable forecasts

The surface variables can be obtained for each forecast date, both for the ensemble (51 members) and deterministic runs.

It includes:

| Parameter name                                                                                 |  ECMWF key | Remarks                         |
|------------------------------------------------------------------------------------------------|------------|---------------------------------|
| [2 metre temperature](https://apps.ecmwf.int/codes/grib/param-db/?id=167)        	             |  2t        |                                 |
| [10 metre U wind component](https://apps.ecmwf.int/codes/grib/param-db/?id=165)	             |  10u       |                                 |
| [10 metre V wind component](https://apps.ecmwf.int/codes/grib/param-db/?id=166)	             |  10v       |                                 |
| [Total cloud cover](https://apps.ecmwf.int/codes/grib/param-db/?id=164)                        |  tcc       |                                 |
| [100 metre U wind component anomaly](https://apps.ecmwf.int/codes/grib/param-db/?id=171006)    |  100ua     |                                 |
| [100 metre V wind component anomaly](https://apps.ecmwf.int/codes/grib/param-db/?id=171007)    |  100va     |                                 |
| [Convective available potential energy](https://apps.ecmwf.int/codes/grib/param-db/?id=59)     |  cape      |                                 |
| [Soil temperature level 1](https://apps.ecmwf.int/codes/grib/param-db/?id=139)                 |  stl1      |                                 |
| [Surface sensible heat flux](https://apps.ecmwf.int/codes/grib/param-db/?id=146)               |  sshf      |                                 |
| [Surface latent heat flux](https://apps.ecmwf.int/codes/grib/param-db/?id=147)                 |  slhf      |                                 |
| [Total column water](https://apps.ecmwf.int/codes/grib/param-db/?id=136)                       |  tcw       |                                 |
| [Total column water vapour](https://apps.ecmwf.int/codes/grib/param-db/?id=137)                |  tcwv      |                                 |
| [Volumetric soil water layer 1](https://apps.ecmwf.int/codes/grib/param-db/?id=39)             |  swvl1     |                                 |
| [Surface net solar radiation](https://apps.ecmwf.int/codes/grib/param-db/?id=176)              |  ssr       |                                 |
| [Surface net thermal radiation](https://apps.ecmwf.int/codes/grib/param-db/?id=177)            |  str       |                                 |
| [Snow depth](https://apps.ecmwf.int/codes/grib/param-db/?id=141)                               |  sd        |                                 |
| [Convective precipitation](https://apps.ecmwf.int/codes/grib/param-db/?id=143)                 |  cp        |                                 |
| [Convective inhibition](https://apps.ecmwf.int/codes/grib/param-db/?id=228001)                 |  cin       |                                 |
| [Surface solar radiation downwards](https://apps.ecmwf.int/codes/grib/param-db/?id=169)        |  ssrd      |                                 |
| [Surface thermal radiation downwards](https://apps.ecmwf.int/codes/grib/param-db/?id=175)      |  strd      |                                 |
| [Visibility](https://apps.ecmwf.int/codes/grib/param-db/?id=3020)                              |  vis       | Observations not available      |

The forecast are available for the model steps (in hours) 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21,
22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55,
56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90,
93, 96, 99, 102, 105, 108, 111, 114, 117, 120, 123, 126, 129, 132, 135, 138, 141, 144, 150, 156, 162, 168, 174, 180, 186, 192, 198,
204, 210, 216, 222, 228, 234 and 240.

**Usage:** The surface variables forecasts can retrieved by calling

``` python
ds = cml.load_dataset('eumetnet-postprocessing-benchmark-training-data-gridded-forecasts-surface', date, parameter, kind)
ds.to_xarray()
```

where the `date` argument is a string with a single date, and the `parameter` argument is a string or a list of string with the
ECMWF keys described above. Setting `'all'` as `parameter` download all the surface parameters. The `kind` argument allows to select 
the deterministic or ensemble forecasts, by setting it to `'highres'` or `'ensemble'`.

**Example:**

``` python
ds = cml.load_dataset('eumetnet-postprocessing-benchmark-training-data-gridded-forecasts-surface', "2017-12-02", "ssr", "highres")
ds.to_xarray()
```

### 1.3 - Pressure level variable forecasts

The variables on pressure level can be obtained for each forecast date, both for the ensemble (51 members) and deterministic runs.

It includes:

| Parameter name                                                                 | Level(s)       |  ECMWF key | Remarks                         |
|--------------------------------------------------------------------------------|----------------|------------|---------------------------------|
| [Temperature](https://apps.ecmwf.int/codes/grib/param-db/?id=130)              |    850         |  t         |                                 |
| [U component of wind](https://apps.ecmwf.int/codes/grib/param-db/?id=131)      |    700         |  u         |                                 |
| [V component of wind](https://apps.ecmwf.int/codes/grib/param-db/?id=132)      |    700         |  v         |                                 |
| [Geopotential](https://apps.ecmwf.int/codes/grib/param-db/?id=129)             |    500         |  z         |                                 |
| [Specific humidity](https://apps.ecmwf.int/codes/grib/param-db/?id=133)        |    700         |  q         |                                 |
| [Relative humidity](https://apps.ecmwf.int/codes/grib/param-db/?id=157)        |    850         |  q         |                                 |

The forecast are available for the same model steps as the surface variables above.

**Usage:** The pressure level variables forecasts can retrieved by calling

``` python
ds = cml.load_dataset('eumetnet-postprocessing-benchmark-training-data-gridded-forecasts-pressure', date, parameter, level, kind)
ds.to_xarray()
```

where the `date` argument is a string with a single date, and the `parameter` argument is a string or a list of string with the
ECMWF keys described above. Setting `'all'` as `parameter` download all the parameters at the given pressure level.
The `level` argument is the pressure level, as a string or an integer. The `kind` argument allows to select
the deterministic or ensemble forecasts, by setting it to `'highres'` or `'ensemble'`.

**Example:**

``` python
ds = cml.load_dataset('eumetnet-postprocessing-benchmark-training-data-gridded-forecasts-pressure', "2017-12-02", "z", 500, "highres")
ds.to_xarray()
```

### 1.4 - Surface variable reforecasts

The surface variables for the ensemble reforecasts (11 members) can be obtained for each reforecast date.
All the variables described at the point **1.2** above are available.

The forecast are available for the model steps (in hours) 0, 6, 12, 18, 24, 30, 36, 42, 48, 54, 60, 66, 72, 78, 84, 90, 96, 102, 108,
114, 120, 126, 132, 138, 144, 150, 156, 162, 168, 174, 180, 186, 192, 198, 204, 210, 216, 222, 228, 234 and 240.

> **Remark:** The ECMWF reforecasts are only available Mondays and Thursdays. Providing any other date provided will fail.

**Usage:** The surface variables reforecasts can retrieved by calling

``` python
ds = cml.load_dataset('eumetnet-postprocessing-benchmark-training-data-gridded-reforecasts-surface', date, parameter)
ds.to_xarray()
```

where the `date` argument is a string with a single date, and the `parameter` argument is a string or a list of string with the
ECMWF keys described above. Setting `'all'` as `parameter` download all the surface parameters. 

**Example:**

``` python
ds = cml.load_dataset('eumetnet-postprocessing-benchmark-training-data-gridded-reforecasts-surface', "2017-12-28", "ssr")
ds.to_xarray()
```

### 1.5 - Pressure level variable reforecasts

The variables on pressure level for the ensemble reforecasts (11 members) can be obtained for each reforecast date
All the variables described at the point **1.3** above are available.

The reforecast are available for the same model steps as the surface variables above.

> **Remark:** The ECMWF reforecasts are only available Mondays and Thursdays. Providing any other date provided will fail.

**Usage:** The pressure level variables reforecasts can retrieved by calling

``` python
ds = cml.load_dataset('eumetnet-postprocessing-benchmark-training-data-gridded-reforecasts-pressure', date, parameter, level)
ds.to_xarray()
```

where the `date` argument is a string with a single date, and the `parameter` argument is a string or a list of string with the
ECMWF keys described above. Setting `'all'` as `parameter` download all the parameters at the given pressure level.
The `level` argument is the pressure level, as a string or an integer.

**Example:**

``` python
ds = cml.load_dataset('eumetnet-postprocessing-benchmark-training-data-gridded-reforecasts-pressure', "2017-12-28", "z", 500)
ds.to_xarray()
```
## 2 - Stations Data

Not yet provided.

## Major model changes

TODO

Support and contributing
------------------------

Please open a [issue on github](https://github.com/Climdyn/climetlab-eumetnet-postprocessing-benchmark/issues).

LICENSE
-------

See the [LICENSE](./LICENSE) file for the code, and the [DATA_LICENSE](./DATA_LICENSE) for the data.

Authors
-------

See the [CONTRIBUTORS.md](./CONTRIBUTORS.md) file.
