# Converting h5 data into cbf for further processing

## FabIO:
- [FabIO](https://fabio.readthedocs.io/en/latest/getting_started.html) is a python module used to read data from x-ray diffraction experiments. FabIO can be used to convert data between file formats
- install FabIO into your environment using a package manager:
```
pip install fabio
```
### Method 1:
- this method is in the case that no specific metadata paramaters need to be added. If you are unsure whether you have the correct metadata for processing with DIALS, skip to method 2.
1. Import the following:
 ```
import h5py
import os
import numpy as np
import hdf5plugin
import fabio
import fabio.utils
```
2. With the **data** variable created in the previous tutorial:
 ```
data = x['/entry/path/to/data/data']
```
3. Use FabIO in a for loop for the whole dataset:
 ```
for image_num in range(0, len(data)):
              
    image = fabio.cbfimage.CbfImage(data[image_num])
    image_num = the_length + image_num
    image.write('images/folder_name/file_name_%05i.cbf' %image_num)
```
Now your data should be saved in a folder called **folder_name** in the format of **file_name_00000.cbf**. This padding with zeroes is necessary as xds requires it to process the data. 

### Method 2:
- in your command line, run the following command:
 ```
python ./eiger2cbf_corr.py -d X -b X -w 0.2776 --omega "0.X*index" -o 'cbf2/file_name{index:04d}.cbf' data_set.h5
```
in which the X behind:
- d is the distance to the crystal
- b is the centre of the detector (needs two coordinates, X and Y)
- w is the wavelength 
- omega is the changing angle frame to frame
- o is the output flag

Now you have a folder containing all of your data with the correct metadata and file format. 
