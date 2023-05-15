# Installation of Meerkat and xds

## Meerkat and Xds work collaboratively in a similar manner to how DIALS compliments Mdx2. 

<details>

<summary>Meerkat</summary>
- [Meerkat](https://github.com/aglie/meerkat) is a python library developed by Arkadiy Simonov. It performs reciprocal space reconstruction from single crystal XRD data. 
- Meerkat can be installed using a package manager such as pip:
  
  ```
pip install meerkat
```

</details> 


<details>

<summary>XDS</summary>
- XDS is needed to run meerkat as it generates an orientation matrix for your dataset. 
- XDS can be downloaded [here](https://wiki.uni-konstanz.de/pub/xds/XDS_html_doc/html_doc/downloading.html). The appropriate Tar file should be downloaded. Once decompressed and untarred, open xds_par, xds_conv and xds to make sure the download was successful. Make a new folder named xds, and move XDS.INP there. The programme is run via this file. 
  
</details> 
