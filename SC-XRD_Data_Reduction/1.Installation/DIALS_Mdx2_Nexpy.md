# Installing DIALS, Mdx2, NeXpy
### Tailored to users who aim to model inorganic materials using crystallographic software

<details>
<summary> DIALS </summary>
  
DIALS is documented on the [DIALS]([url](https://dials.github.io/index.html)) github. DIALS is software used for indexing, peak finding, scaling and various other functions found [here](https://dials.github.io/index.html)), it is run entirely from the command line and has a basic UI to view data. DIALS should be installed in a clean environment and runs best on Python 3.10. DIALS can be installed using a package manager such as conda:
  
  ```
conda install -c conda-forge dials
conda install dxtbx/n # to read image headers 
```

</details>

<details>
<summary> Mdx2 </summary>
Mdx2 was released in 2022 at the Erice Data Reduction Workshop by the International School of Crystallography. Mdx2 is a valuable package for data analysis and to generate reciprocal space plots. 
Mdx2 can be downloaded from:  https://github.com/ando-lab/mdx2/archive/refs/tags/v0.3.0.zip
 First unzip and then move to the same directory as the dataset, then in the command line, run:
  
  ```
pip install -e
```
Subsequently, run:
  
  ```
mdx2.version
```
This should output the installed version of mdx2. 
  
</details>

<details>
<summary> NeXpy </summary>
NeXpy is the GUI that accompanies Mdx2. NeXpy should be installed in a second environment running on python 3.9 (or the last stable release). NeXpy only processes files in a NexusFormat structure (.nxs). Hierarchal data formats such as .h5 can be converted to the NexusFormat using this [script](https://github.com/cammdu/crystal-toolbox/blob/main/mdx2/h5_to_nexpy.md)
NeXpy can be installed using conda:

   ```
conda install -c conda-forge nexpy
conda install scipy  
``` 
  
</details>
