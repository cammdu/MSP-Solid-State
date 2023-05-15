# Creating a Mask in Python
#### The steps below outline how to make a mask in python and how to apply the mask to the dataset. 
1. Go to your command line in your python 3.9 environment:
2. open python
```
python
```
3. import the following:
```
import h5py
importhdf5plugin
import numpy as np
```
4. make your file a variable (as seen in pre_processing)
```
f = h5py.File('h5_file_name.h5','r')
```
5. show the path to data, which we know because it was defined in pre_processing:
```
data = f['entry/path/to/data/data']
```
6. Print the shape of your data using:
```
data.shape
```
- this should output the shape of your data as X,Y,Z coordinates

7. define a frame as:
 ```
frame = data[0]
```
8. Now sum frames less than 0 (as a check, should be 0)
 ```
np.sum(frame<0)
```
9. Now define a threshold for your mask. This count threshold limit is to eliminate hot pixels. A count threshold can be calculated by multiplying the standard deviation of the background intensities by 10 (if your data has very few hot pixels). If your data has many hot pixels of a very high intensity, then your count threshold should be much higher. In my case, the threshold was 10,000 which is quite high. 
 ```
threshold = 10000
```
10. Now as another check, sum how many frames have an intensity above 10,000
 ```
np.sum(frame>threshold)
```
- this will output a number, if this number is very small, then consider decreasing your count threshold value. If this output is large, move on to the step below.
11. Checking whether the previous output is reasonable by computing min values of the first 100 rows of data
 ```
minframe = np.min(data[:100], axis = 0)
np.sum(minframe>threshold)
```
- if this returns a similar value to step 10, we know that the threshold is reasonable. 
12. Defining pixels we consider not hot (below the defined threshold):
 ```
not_hot = minframe<threshold
```
13. Define our output:
 ```
out = h5py.File('mask.h5','w') #w because we are writing the mask
out = ['data'] = not_hot
out.close()
```
Now the mask has been created. 

## Applying the mask to the reciprocal space reconstruction:
#### make sure that you have read the meerkat tutorials as we will be using the meerkat reconstruction for this. 
1. add the lines cushioned in ** to the following code:
 ```
**import h5py**
from meerkat import reconstruct_data
**mask_f = h5py.File('mask.h5','r')**
**measured_pixels = mask_f['data'][()]**
reconstruct_data(filename_template='../frames/file_name_%05i.cbf',
        first_image=1,
        last_image=XXXX,
        reconstruct_in_orthonormal_basis=False,
        **measured_pixels = measured_pixels,**
        maxind=[4,5,16], #the reconstruction will be made for h=-4...4, k=-5...5, l=-16...16
        number_of_pixels=[801, 801, 801], #The resulting size of the array. Controls the step size
        polarization_factor=0.5,
        path_to_XPARM='/home/arkadiy/work/data/PdCPTN01002/xds',
        output_filename='reconstruction.h5',
        all_in_memory=True, #change to false if memory does not allow
        size_of_cache=100,
        override=True)
```
Run this reconstruction followed by the show_date python file (tutorial_meerkat). Your mask should now be applied to the reciprocal space reconstruction. Well done!
