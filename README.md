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


## Thoughts on data correction library

We could start building a Python library based on the ESMValTool using xarray providing known fixes.

* Looking at the ESMValTool library the number of fixes might be like hundreds (mapped to metadata like models etc) ... not millions.
* Is a registration database necessary?
* Do we just use a database for quick solution lookups ... using sources maintained on GitHub (json/yaml, Python, ...)?
* The data correction can also be provided as a WPS process.

Ouranos likes to have a *virtual data layer* with fixes on top of the existing files using NcML and Thredds:

* NcML would only fix metadata issues ... but ESMValTool already has fixes which adapts unit values ... maybe the data itself (?).
* In the *Future ESGF architecture* we would like to skip Thredds and use Nginx (for simple file downloads).
* Is it possible to extend Nginx for a *virtual data layer*?
* Could we use FUSE (Filesystem in User Space) to provide a *virtual data layer*? Nginx (etc) will just see a normal filesystem.
* If the *virtual data layer* might work it will change the data on each download request. We might want to have some sort of caching.

Advantages of a virtual data layer:

* All operations do not need to care about data corrections.
* We might not need to provide an additional data-pool like C3S-CORDEX. Fixes will be applied to the library.
* There is no *data sync* step for new fixes. Just update the virtual data layer.

## Thoughts on Operators

The xclim library by ouranos contains subsetting with xarray for bbox, shape, time, ...:
https://github.com/Ouranosinc/xclim/blob/master/xclim/subset.py

We could use this code as a staring point.

So far we are missing examples where we know that the operators need to behave different depending on the input data (cmip6, cordex, 2d, 3d, ..., ?)

What is the situation with re-gridding?

## Thoughts on Testing

We can use *pytest* to run a test-suite.

* Should we run against some fixed input data (only a few MBs large) ... possible also on Travis CI?
* Should we run it *brute-force* against a full CORDEX data pool?

How do we identify that the operation was correct?
* output data should be like the input data except for the changes by the operation.
* ???

## Thoughts on the WPS service

How do we define the inputs?
* file URLs ...
* a description with metadata: model, institute, ...
* an OpenSearch search query ...
* ... brings us back to our meta WPS service.

How to we return the output?
* should we extend the file metadata with the operation?
* what are the file naming conventions ... `orig_name_subset.nc`?
* do we return a single file?
* multiple files can be returned for example with metalink:
https://pywps.readthedocs.io/en/latest/process.html#returning-multiple-files
