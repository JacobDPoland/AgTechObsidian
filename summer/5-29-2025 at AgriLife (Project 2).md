# Structure from motion and photogrammetry (SfM)
## What is SfM?
- Photogrammetric technique used to generate 3d models from a series of 2d images
## Key steps- Image Capture
- Image Capture
- Feature extraction
- Camera calibration
- Bundle adjustment
- 3d reconstruction
## SfM Workflow
1. Image capture
2. Image processing
3. Point cloud generation (LiDAR gives this)
4. 3d model generation
## Applications of SfM and Photogrammetry
- 3d modeling of buildings, landscapes, and objects
- Archaeology
- Surveying and mapping
- Industrial inspection and reverse engineering
- Urban planning
- Conservation
## Advantages of SfM
- Low cost data acquisition
- Flexible and adaptive workflow
- Accurate and detailed models
--- 
## Unmanned Aircraft Systems (UAS Manual) by Texas A&M AgriLife Research
We used the above mentioned manual while attending following demonstrations and tutorials.
## MetaShape Demonstration and Tutorial
We covered:
- Structure from Motion (SfM) in Agisoft Metashape on Page 42 of the manual
- 

## QGIS Demonstration and Tutorial
- Be careful how plot boundaries are chosen
We covered:
- Creating Plot boundaries on page 51 of the manual
- Calculating NDVI from Separate Bands on Page 60 of the manual
- Exporting Excel files from Shapefile on page 67
### Band numbers from the drone's sensor: 
1 is Blue
2 is Green 
3 is Panchro (panchromatic imagery) 
4 is Red 
5 is Red Edge
6 is NIR 
7 is LWIR (Long Wavelength Infrared) 
8 is Alpha (can be excluded for data analysis) 

### Plot boundary tool plugin for QGIS
Plugins > manage and install > search for plot boundary by Jinha Jung