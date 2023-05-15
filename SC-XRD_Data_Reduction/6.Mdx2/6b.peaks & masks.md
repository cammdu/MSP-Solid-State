# Processing Image Data
## Before starting this tutorial make sure you have geometry.nxs
### Preliminary 
#### note: Background processing is not covered here. If this is relevant to you, please be referred to the [Erice-2022-Data-Reduction-Workshop](https://github.com/ando-lab/erice-2022-data-reduction/blob/main/3_mdx2_data.ipynb)
To process data in mdx2, the data can be sliced to increase processing speed. If you aim to process the 360 degrees, skip this step.  Choose an image range that is suitable for you (has to fit properly into your dataset). 
```
dials.slice_sequence refined.expt "image_range=X Y"
```
- where X is your lower limit frame and Y is your upper limit frame. This can be any range that is suitable for your data set. 
- If your dataset is 360 degrees, subsets of 50 degrees work well, e.g.:
```
dials.slice_sequence refined.expt "image_range=100 600"
```
- this will output refined_100_600.expt and save it in the previously designated directory. 

### Processing in mdx2
1. In an IDE or in python on the terminal:
```
from nexusformat.nexus import nxload
```
2. Proceed with importing your data:
```
mdx2.import_data refined.expt --chunks X Y Z
```
- where X,Y and Z are parameters for the chunks in which you read data. Smaller chunks are great as they increase processing efficiency. 
- this will output data.nxs

3. To visualise your data, open data.nxs in NeXpy by running
```
nexpy
```
in your terminal. 
- In NeXpy find: **open --> data.nxs**
- please note that data.nxs is a very large file (>3gb). Therefore it is recommended that inspecting data.nxs is only done on a device with adequate space.
- in data.nxs you can go through the 'z' slices and observe different bragg peaks. 
- this is a good opportunity to see if the mask applied using dials is properly masking hot pixels. 

4. [Erice-2022 data reduction workshop](https://github.com/ando-lab/erice-2022-data-reduction/blob/main/3_mdx2_data.ipynb) demonstrates that it is possible to inspect whether there is any diffuse scattering available in your data using **data.nxs** 
- You can use matplotlib to plot a strong peak (not a hot pixel) and then plot a rocking curve to see if there is any diffuse scattering or if the peak is sharp.  
- make this plot logarithmic to properly see the signal of the peak:
```
plot_peak = nxload('data.nxs')['/entry/image_series']
plot_peak.project(0,limits=((A,B),(C,D),(E,F))).logplot()
```
- where A,B are how many values should be displayed on your plot
- where C,D are the min,max y axis coordinates
- where E,F are the min, max x axis coordinates 

## Masking
1. During masking, it is very important to be familiar with your data as you need to determine a count threshold for hot pixels. 
- There are many methods for doing so, but not every method is applicable to every dataset. If your data has high mosaicity, then a lower count threshold is wise, whereas if your data is highly symmetric, you can consider a much higher count threshold. 
```
mdx2.find_peaks geometry.nxs data.nxs --count_threshold X
```
- where X is your chosen count threshold (integer). 
- the function will proceed to find every pixel with counts above the specified threshold. 

2. the next step is to determine your sigma cutoff value. This value essentially demonstrates how much confidence you have in the reconstruction. 
- The sigma cutoff is therefore a number of standard deviations from the mean electron density. 
- If your data is high quality, a lower sigma cutoff value should be used as this sigma cutoff determines the sizes of the spots which make the mask: i.e. a large sigma cutoff (3) masks much more of your data than a low sigma cutoff (0.5). 
```
mdx2.mask_peaks geometry.nxs data.nxs --sigma_cutoff X.Y
```
- where X.Y is your chosen sigma cutoff value
- this will output **mask.nxs**, if you open this in NeXpy, you can visualise what a mask looks like for your data. 
