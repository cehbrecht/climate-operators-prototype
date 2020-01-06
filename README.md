# climate-operators-prototype
Prototype for reliable Climate Operators

## Copernicus Reliable and Robust Operators

* WPS service interface
* basic operators: subsetting, regridding, ...
* climate model data: cordex, cmip5, cmip6
* using xarray
* uses a registry to maintain data fixes and operator solutions

### Registry

Registry could be a database tracking issues and solutions:
* https://dbdiagram.io/home
* https://dbdiagram.io/d/5dea43fcedf08a25543eca8f

### Operators

Subsetting with xarray:
* http://xarray.pydata.org/en/stable/

Regridding with xesmf:
* https://xesmf.readthedocs.io/en/latest/

## ESMValTool data correntions

ESMValTool has a library of known data corrections:
* https://github.com/ESMValGroup/ESMValCore/tree/master/esmvalcore/cmor/_fixes
* https://github.com/ESMValGroup/ESMValCore/blob/master/esmvalcore/cmor/_fixes/cmip5/access1_0.py#L33

## Data services with fixes

Ouranos likes to have a data service layer on top of this original data pool with fixes.

Using ncml:
* https://github.com/axiom-data-science/pyncml
* https://github.com/NCAR/xncml

Collection issues:
* https://github.com/Ouranosinc/pavics-vdb/issues
