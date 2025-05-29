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
## Vegative Index Notes From Seminar Slide
[[4-9-2025 Plant Physiology 101 - Dr. Jose Landivar]]
# 🌿 MS Vegetative Indexes
These indices help assess plant health, biomass, and vegetation density using multi-spectral satellite imagery or drone data.

---
## 1. **NDVI** – _Normalized Difference Vegetation Index_
**Equation:**
ini
CopyEdit
`NDVI = (N - R) / (N + R)`
- **N** = Near-infrared reflectance
- **R** = Red reflectance
**Purpose:** Measures live green vegetation. Higher NDVI = more vegetation.
---
## 2. **NDRE** – _Normalized Difference Red Edge Index_
**Equation:**
mathematica
CopyEdit
`NDRE = (N - E) / (N + E)`
- **E** = Red edge reflectance
**Purpose:** Useful for estimating chlorophyll content and detecting stress before it appears visibly.
---
## 3. **GNDVI** – _Green Normalized Difference Vegetation Index_
**Equation:**
ini
CopyEdit
`GNDVI = (N - G) / (N + G)`
- **G** = Green reflectance
**Purpose:** Better than NDVI for detecting vegetation stress due to higher sensitivity to chlorophyll.
---
## 4. **SAVI** – _Soil Adjusted Vegetation Index_
**Equation:**
ini
CopyEdit
`SAVI = ((N - R) * (1 + α)) / (N + R + α)`
- **α** = Soil brightness correction factor (typically 0.5)
**Purpose:** Improves NDVI by correcting for soil brightness influence.
---
## 5. **OSAVI** – _Optimized Soil Adjusted Vegetation Index_
**Equation:**
ini
CopyEdit
`OSAVI = SAVI (α = 0.16)`
- Uses a smaller α value (0.16) for better performance in areas with less vegetation.
**Purpose:** A more refined version of SAVI with optimized α for sparse vegetation.
---
## 6. **MSAVI** – _Modified Soil Adjusted Vegetation Index_
**Equation:**
mathematica
CopyEdit
`MSAVI = (2N + 1 - sqrt((2N + 1)² - 8(N - R))) / 2`
**Purpose:** Further adjusts for soil brightness without needing a fixed α value. Ideal for areas with very low vegetation cover.
---
## 7. **GCI** – _Green Chlorophyll Index_
**Equation:**
ini
CopyEdit
`GCI = (N / G) - 1`
**Purpose:** Estimates the chlorophyll content in plants, especially in high vegetation cover.

---
## 8. **RECI** – _Red Edge Chlorophyll Index_
**Equation:**
ini
CopyEdit
`RECI = (N / E) - 1`
**Purpose:** Like GCI, but uses red edge data for better detection of subtle chlorophyll changes.

---
## Summary Table
| Index | Equation                             | Key Bands Used | Purpose                                |
| ----- | ------------------------------------ | -------------- | -------------------------------------- |
| NDVI  | (N - R) / (N + R)                    | NIR, Red       | General vegetation health              |
| NDRE  | (N - E) / (N + E)                    | NIR, Red Edge  | Chlorophyll and early stress detection |
| GNDVI | (N - G) / (N + G)                    | NIR, Green     | Stress detection via chlorophyll       |
| SAVI  | ((N - R)(1 + α)) / (N + R + α)       | NIR, Red       | Vegetation + soil adjustment           |
| OSAVI | SAVI with α = 0.16                   | NIR, Red       | Improved SAVI for sparse vegetation    |
| MSAVI | (2N + 1 - √((2N + 1)² - 8(N - R)))/2 | NIR, Red       | Soil-corrected index (no α needed)     |
| GCI   | (N / G) - 1                          | NIR, Green     | Chlorophyll estimation                 |
| RECI  | (N / E) - 1                          | NIR, Red Edge  | Chlorophyll with red edge sensitivity  |
