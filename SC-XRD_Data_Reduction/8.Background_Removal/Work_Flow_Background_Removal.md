# A Simplified Work Flow for Background Subtraction (DIALS & Mdx2 only)
### Requirement: Background dataset (of a capillary or any other background)
#### note: for an explanation of what individual commands do, as well as how to install the software and use XDS be referred to tutorials 1-7.
1. Import XPARM.XDS and XDS.INP
```
dials.import_xds ../xds_
```
2. Peak spotting wit DIALS
```
dials.find_spots xds_models.expt
```
3. Index the strong reflections
```
dials.index xds_models.expt strong.refl
```
4. Refine the indexing
```
dials.refine indexed.expt indexed.refl
```
- if this assigns the proper unit cell, skip steps 5
5. further refining. Repeat as many times as needed:
```
dials.refine_bravais_settings indexed.expt indexed.refl
```
```
dials.reindex bravais_setting_XX.expt space_group=SPC_GRP
```
```
dials.index reindexed.expt strong.refl
```
```
dials.refine_bravais_settings indexed.expt indexed.refl
```
```
dials.refine indexed. expt indexed.refl
```
6. edit refined.expt if your exposure time is 0. Do this with a simple for loop which substitutes 0.0 with your exposure time. The exposure time can be found in the metadata of the files (can be seen easily using a program such as ALBULA
7. Statistical plots and important information about your dataset can be found using:
```
dials.report refined.expt refined.refl
```
8. Import your background images
```
dials.import ../cbf_background output.experiments=background.expt
```
9. Import the geometry from DIALS to Mdx2

```
mdx2.import_geometry refined.expt
```
10. Look at the file specs
```
mdx2.tree geometry.nxs
```
11. Import the data from DIALS
```
mdx2.import_data refined.expt --chunks 120 120 120
```
12. Import the background data from DIALS
```
mdx2.import_data background.expt --outfile bkg_data.nxs --chunks 120 120 120
```
13. Bin the background image stack
```
mdx2.bin_image_series bkg_data.nxs XX XX XX --valid_range XX XX --outfile bkg_data_binned.nxs
```
14. Find peaks 
```
mdx2.find_peaks geometry.nxs data.nxs --count_threshold 10000
```
15. Mask
```
mdx2.mask_peaks geometry.nxs data.nxs peaks.nxs --sigma_cutoff 3
```
16. Integration    
```
mdx2.integrate geometry.nxs data.nxs --mask mask.nxs --subdivide 7 7 7
```
17. Corrections 
```
mdx2.correct geometry.nxs integrated.nxs --background bkg_data_binned.nxs
```
18. Merge
```
mdx2.merge corrected.nxss --outfile merged_not_scaled.nxs
```
19. Scale
```
mdx2.scale corrected.nxs
```
20. Make a 2D slice
```
mdx2.map geometry.nxs merged.nxs --limits XX XX XX XX XX XX --outfile slice.nxs
```
21. Make a complete 3D reciprocal space reconstruction
```
mdx2.map geometry.nxs merged.nxs --limits XX XX XX XX XX XX --outfile map.nxs
```
22. Open in nexpy
```
nexpy
```
