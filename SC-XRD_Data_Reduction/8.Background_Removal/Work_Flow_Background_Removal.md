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
dials.refine indexed. expt indexed.refl
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























