## Author: Srikanth Panthulugiri
**Last Updated:** 12/15/2025 <br>
**Project Name:** Spatial Detection of Flood Stress in Soybean Using ArcGIS <br>

---

## Overview
This repository contains a two-stage geospatial processing pipeline used to analyze temporal changes in vegetation indices derived from multispectral imagery. The workflow processes individual spectral bands collected at multiple timepoints, computes vegetation indices, spatially aligns them, and generates final comparison heatmaps and cumulative distribution function (CDF) plots.

---

## Folder Structure

```
GIS_final_project
│
├── Resampled_Raw_Data_1m_resolution
│   ├── 2025_TP1
│   │   ├── resampled_20250715_mosaic_green.tif
│   │   ├── resampled_20250715_mosaic_red.tif
│   │   └── resampled_20250715_mosaic_nir.tif
│   │
│   ├── 2025_TP2
│   └── 2025_TP3
│
├── Resampled_Veg_Indices
│   ├── TP1_Indices
│   │   ├── TP1_20250715_NDVI.tif
│   │   └── TP1_20250715_NDRI.tif
│   │
│   ├── TP2_Indices
│   │   ├── TP2_20250812_NDVI.tif
│   │   └── TP2_20250812_NDRI.tif
│   │
│   └── TP3_Indices
│       ├── TP3_20250825_NDVI.tif
│       └── TP3_20250825_NDRI.tif
│
├── Temporal_Analysis_Output
│   ├── plots
│   │   ├── NDVI_heatmap.png
│   │   ├── NDVI_CDF.png
│   │   ├── NDRI_heatmap.png
│   │   └── NDRI_CDF.png
│
├── TB_stack_clip_indices_rasters.pyt
├── 3_heatmaps_png.py
└── README.md
```

---

## Inputs

### Individual Band Rasters (per timepoint)
Each timepoint folder contains single-band, resampled raster files:

```
resampled_<DATE>_mosaic_<band>.tif
```

Required bands:
- green
- red
- nir

All rasters:
- Are projected
- Share the same coordinate system
- Are resampled to 1 m × 1 m resolution prior to analysis

---

## Step 1 — Toolbox Processing (Per Timepoint)

**Tool:** `TB_Multispectral_AgriPipeline.pyt`

**Applied to:**  
Each individual timepoint folder in `Resampled_Raw_Data_1m_resolution`.

**Operations performed:**
1. Stack individual spectral bands into a multiband raster
2. Clip the raster to a field boundary shapefile
3. Compute vegetation indices:
   - NDVI = (NIR − Red) / (NIR + Red)
   - NDRI = (Red − Green) / (Red + Green)
4. Save outputs to `Resampled_Veg_Indices/TPx_Indices`

**Outputs per timepoint:**
- `TPx_<DATE>_NDVI.tif`
- `TPx_<DATE>_NDRI.tif`

---

## Step 2 — Temporal Analysis & Visualization

**Script:** `3_heatmaps_png.py`

**Input folder:**  
```
Resampled_Veg_Indices
```

**Key processing steps:**
1. Automatically detect TP1, TP2, TP3 folders
2. Load NDVI and NDRI rasters dynamically
3. Use TP1 as the reference grid
4. Resample TP2 and TP3 to TP1 resolution
5. Clip all rasters to a common spatial extent
6. Convert rasters to NumPy arrays
7. Compute temporal differences:
   - TP2 − TP1
   - TP3 − TP2
   - TP3 − TP1

---

## Final Outputs

### Heatmaps
Spatial visualization of temporal change:
- NDVI_heatmap.png
- NDRI_heatmap.png

### CDF Plots
Pixel-level distribution comparisons:
- NDVI_CDF.png
- NDRI_CDF.png

All plots are saved in:
```
Temporal_Analysis_Output/plots
```

---

## Software & Libraries
- ArcGIS Pro (ArcPy, Spatial Analyst, 3D Analyst)
- Python (NumPy, Matplotlib)

---

## Summary Workflow

```
Individual Bands
        ↓
Toolbox: Stack + Clip + NDVI/NDRI
        ↓
Resampled_Veg_Indices
        ↓
3_heatmaps_png.py
        ↓
Final Temporal Comparison PNGs
```

---

## Notes
This workflow is suitable for GIS coursework, research-grade temporal analysis, and reproducible spatial comparison studies.
