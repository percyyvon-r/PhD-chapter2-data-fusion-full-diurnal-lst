# PhD Chapter 2: Data Fusion for Full Diurnal Land Surface Temperature Mapping

## Overview

This repository contains the complete workflow for a spatiotemporal data fusion study aimed at retrieving continuous high-resolution Land Surface Temperature (LST) data during peak summer months (December-February) over Port Philip Bay. The project implements two state-of-the-art fusion algorithms and provides comprehensive accuracy assessments.

## Project Objectives

- Develop high-resolution diurnal (day and night) LST maps using multiple satellite data sources
- Implement and compare spatiotemporal data fusion techniques (STARFM and FSDAF)
- Assess fusion accuracy using multiple metrics including SSIM, RMSE, and correlation coefficients
- Demonstrate temporal stability and spatial pattern consistency of fused LST products

## Data Sources

| Data Type | Source | Resolution | Period | Processing |
|-----------|--------|-----------|--------|------------|
| Coarse Daytime (Day) | MODIS | 1 km | Peak summer | APPEEARS, OSGeo4W Shell |
| Coarse Daytime (Night) | MODIS | 1 km | Peak summer | APPEEARS, OSGeo4W Shell |
| Fine Daytime | Landsat 8/9 | 30 m | Peak summer | Google Earth Engine |
| Fine Nighttime | ECOSTRESS | 70 m | Peak summer | APPEEARS, ArcGIS |
| Sea Surface Reference | Himawari SST | 2 km | Peak summer | WinSCP |

## Repository Structure

```
.
├── raw/                          # Raw satellite data
│   ├── day/                       # Daytime raw data (MODIS, Landsat)
│   ├── night/                     # Nighttime raw data (MODIS, ECOSTRESS)
│   └── fsdaf_classification/      # Land cover classification inputs
├── processed/                     # Intermediate processing outputs
├── output/                        # Final analysis outputs
│   ├── raster/                    # Fused LST raster files
│   │   ├── day/                   # Daytime fusion results
│   │   └── night/                 # Nighttime fusion results
│   ├── csv/                       # Accuracy metrics in CSV format
│   └── plots/                     # Visualization outputs
├── shapefile/                     # Spatial data and study area definitions
├── fsdaf_parameters/              # FSDAF algorithm parameter files
│   ├── parameters_fsdaf.yaml
│   ├── parameters_fsdaf_preclassification_day.yaml
│   └── parameters_fsdaf_preclassification_night.yaml
├── accuracy_assessment/           # Accuracy assessment scripts and results
│   ├── optimal_accuracy_metrics.py
│   ├── APA_diagram.R
│   └── Fusion-accuracy-assessment-main/
├── notebook/                      # Jupyter notebooks for analysis
│   └── fused_lst_temporalStability_spatialPatternConsistency.ipynb
└── preprocessing/                 # Data preprocessing documentation and scripts
```

## Key Methodologies

### 1. Data Preprocessing

- **Mosaicking**: Combining multiple tiles into seamless datasets
- **Reprojection**: Aligning all datasets to UTM projection
- **Resampling**: Standardizing spatial resolution (2 km → 1 km → 30 m for day; 1 km → 70 m for night)
- **Spatial Extraction**: Using fishnet grids and masking to extract study area
- **Extrapolation**: IDW (Inverse Distance Weighted) interpolation for filling data gaps

### 2. Spatiotemporal Data Fusion

#### STARFM (Spatial and Temporal Adaptive Reflectance Fusion Model)
- Reference: [starfm4py](https://github.com/nmileva/starfm4py)
- Blends coarse temporal resolution and fine spatial resolution data
- Maintains temporal consistency while enhancing spatial detail

#### FSDAF (Spatial and Temporal Data Adaptive Fusion)
- Reference: Zhu et al., 2016
- Uses land cover classification to improve fusion accuracy
- Pre-classification approach uses RF (Random Forest) classification
- Separate daytime (14/01/2023) and winter (31/08/2022) classifications
- Cloud-free pixel selection for reference data quality

### 3. Accuracy Assessment

Multiple accuracy metrics were computed following Zhu et al. (2022):

| Method | Inputs | Purpose |
|--------|--------|---------|
| STARFM | Fine reference + Fused image | Assess STARFM fusion accuracy |
| FSDAF | Fine reference + Fused image | Assess FSDAF fusion accuracy |
| F0 | Fine reference + Fine base image | Assess fine data temporal change |
| CP | Fine reference + Coarse reference | Compare resolution effects |
| ATF | Amplified temporal component | Test temporal sharpening |
| ASF | Amplified spatial component | Test spatial sharpening |

**Metrics Computed**:
- Structural Similarity Index (SSIM)
- Root Mean Square Error (RMSE)
- Pearson Correlation Coefficient (R)
- Mean Absolute Error (MAE)
- Bias

## Main Outputs

### Analysis Notebook
The primary analysis is contained in:
- `notebook/fused_lst_temporalStability_spatialPatternConsistency.ipynb`

This comprehensive Jupyter notebook includes:
1. Data loading and exploration
2. Temporal stability analysis across fusion methods
3. Spatial pattern consistency assessment
4. Comparative visualizations (Taylor diagrams, time series plots)
5. Accuracy metrics comparison

### Visualization Results
- Comparison images of observed vs. predicted LST
- Temporal trend plots and time series analysis
- Spatial pattern maps for different fusion methods
- Accuracy assessment visualizations (Taylor diagrams)

## Dependencies

- **Python 3.7+**
- **Geospatial Libraries**: GDAL, rasterio, shapely
- **Scientific Computing**: NumPy, SciPy, Pandas, scikit-image
- **Visualization**: Matplotlib, Jupyter
- **Data Analysis**: scikit-learn

Install dependencies:
```bash
pip install -r requirements.txt
```

## Reference Studies

1. **Zhu et al. (2022)** - Accuracy assessment methods
   - DOI: https://doi.org/10.1016/j.rse.2022.112944

2. **Zhu et al. (2016)** - FSDAF Algorithm
   - FSDAF for spatiotemporal data fusion

3. **Ermida et al. (2020)** - Landsat LST Processing
   - GitHub: https://github.com/sofiaermida/Landsat_SMW_LST

4. **Mileva et al. (2018)** - STARFM Implementation
   - GitHub: https://github.com/nmileva/starfm4py

## Study Area

- **Location**: Port Philip Bay, Australia
- **Period**: Peak summer months (December - February)
- **Analysis Years**: 2021-2023

## How to Use

### 1. Data Acquisition
Follow the preprocessing documentation for each data source:
- MODIS data: See `preprocessing/modis.txt`
- Himawari SST: See `preprocessing/himawari.txt`
- Landsat 8/9: Use Google Earth Engine script (Ermida et al., 2020)
- ECOSTRESS: Download from APPEEARS

### 2. Data Preprocessing
Execute preprocessing scripts in order:
```bash
# For daytime processing
# See: day_preprocessing.txt

# For nighttime processing
# See: preprocessing/nighttime_preprocessing.txt
```

### 3. Run Fusion Algorithms
- **STARFM**: Use parameters in preprocessing documentation
- **FSDAF**: Configure parameters in `fsdaf_parameters/` directory

### 4. Analyze Results
Run the main analysis notebook:
```bash
jupyter notebook notebook/fused_lst_temporalStability_spatialPatternConsistency.ipynb
```

## Author

Percy Yvon-Boucher  
PhD Researcher, Chapter 2 Study

## License

[Specify your license here, e.g., MIT, GPL, or Academic Use Only]

## Acknowledgments

- NASA APPEEARS for MODIS and ECOSTRESS data access
- Google Earth Engine for Landsat processing platform
- Prof. [Advisor Name] and research group for guidance and support
- Zhu et al. for accuracy assessment methodology and code

## Contact

For questions or inquiries about this research, please contact:
[Your email]

## Citation

If you use this research or code in your work, please cite:

```bibtex
@misc{yvonboucher2024,
  author = {Yvon-Boucher, Percy},
  title = {PhD Chapter 2: Data Fusion for Full Diurnal Land Surface Temperature Mapping},
  year = {2024},
  type = {PhD Research Repository},
  url = {https://github.com/percyyvon-r/PhD-chapter2-data-fusion-full-diurnal-lst}
}
```

## Project Status

- Data Collection: ✅ Complete
- Preprocessing: ✅ Complete
- Fusion Processing: ✅ Complete
- Accuracy Assessment: ✅ Complete
- Analysis & Visualization: 🔄 In Progress
- Final Publication: ⏳ Planned
