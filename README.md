# Grib2Reader

 Importer for NSSL's Multi-Radar/Multi-Sensor System
    ([MRMS](https://www.nssl.noaa.gov/projects/mrms/)) rainrate product
    (grib format).

The rainrate values are expressed in mm/h, and the dimensions of the data
    array are [latitude, longitude]. The first grid point (0,0) corresponds to
    the upper left corner of the domain, while (last i, last j) denote the
    lower right corner.

Due to the large size of the dataset (3500 x 7000), a float32 type is used
    by default to reduce the memory footprint. However, be aware that when this
    array is passed to a pystep function, it may be converted to double
    precision, doubling the memory footprint.
    To change the precision of the data, use the *dtype* keyword.

Also, by default, the original data is not downscaled but can be done by setting `window_size=4`
    (resulting in a ~4 km grid spacing).
    In case that the original grid spacing is needed, use `window_size=1`.
    But be aware that a single composite in double precipitation will
    require 186 Mb of memory.

Finally, if desired, the precipitation data can be extracted over a
    sub region of the full domain using the `extent` keyword.
    By default, the entire domain is returned.

### Notes

In the MRMS grib files, "-3" is used to represent "No Coverage" or
    "Missing data". However, in this reader replace those values by the value
    specified in the `fillna` argument (NaN by default).

Note that "missing values" are not the same as "no precipitation" values.
    Missing values indicates regions with no valid measures.
    While zero precipitation indicates regions with valid measurements,
    but with no precipitation detected.

### Parameters

    filename: str
        Name of the file to import.
    extent: None or array-like
        Longitude and latitude range (in degrees) of the data to be retrieved.
        (min_lon, max_lon, min_lat, max_lat).
        By default (None), the entire domain is retrieved.
        The extent can be in any form that can be converted to a flat array
        of 4 elements array (e.g., lists or tuples).
    window_size: array_like or int
        Array containing down-sampling integer factor along each axis.
        If an integer value is given, the same block shape is used for all the
        image dimensions.
        Default: window_size=4.

    {extra_kwargs_doc}

### Returns
    precipitation: 2D array, float32
        Precipitation field in mm/h. The dimensions are [latitude, longitude].
        The first grid point (0,0) corresponds to the upper left corner of the
        domain, while (last i, last j) denote the lower right corner.
    quality: None
        Not implement.
    metadata: dict
        Associated metadata (pixel sizes, map projections, etc.).
