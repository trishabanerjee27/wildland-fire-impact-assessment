# wildland-fire-impact-assessment

This repository contains code and data for assessing the impact of wildland fires on air quality using historical wildfire data, smoke estimates, and AQI metrics. The project utilizes multiple datasets and predictive models to forecast future smoke exposure and its correlation with AQI (Air Quality Index) over time. The primary objective is to understand the spatial and temporal patterns of wildfires and their potential effects on urban air quality.

### Repository Structure
```
wildland-fire-impact-assessment
├── code
│   └── part 1 analysis.ipynb      # Jupyter notebook for data analysis and modeling
├── data
│   ├── derived                    # Processed and derived datasets for analysis
│   │   ├── aqi_estimate_v1.csv
│   │   ├── average_aqi_data_by_year_v3.csv
│   │   ├── raw_aqi_data_v7.csv
│   │   └── xml_to_csv.xlsx
│   ├── raw                        # Raw datasets directly from external sources
│   │   ├── Fire_Feature_Data.gdb
│   │   ├── raw_aqi_data_v6.csv
│   │   └── Wildland_Fire_Polygon_Metadata.xml
├── images                         # Visualizations generated from the analysis
│   ├── fires_distance_histogram.png
│   ├── smoke_estimate_forecast_sarima.png
│   ├── time_series_fire_smoke_aqi_estimates.png
│   └── total_acres_burned.png
├── .gitignore                     # Git ignore file for managing ignored files
├── LICENSE                        # License file for the project
└── README.md                      # Project README file (you are here)

```

### Folders and Files

- **`code/`**: Contains the main Jupyter notebook `part 1 analysis.ipynb` where data loading, processing, modeling, and visualization are conducted.
- **`data/derived/`**: Stores cleaned, processed, and derived datasets used for analysis. Files here include various versions of AQI and smoke estimate data, created during different stages of data processing.
- **`data/raw/`**: Contains original datasets like `Fire_Feature_Data.gdb` and `Wildland_Fire_Polygon_Metadata.xml`, which provide wildfire information, and `raw_aqi_data_v6.csv`, a version of the AQI dataset before transformations.
- **`images/`**: Holds visualizations created from the analysis, such as histograms, forecasts, and time series plots that depict fire distance distributions, smoke estimate forecasts, and trends in acres burned.
- **`.gitignore`**: Specifies files and folders to exclude from version control.
- **`LICENSE`**: License for the repository.
- **`README.md`**: This file, providing a detailed overview of the project.

## Project Overview

### Objectives

The primary goal of this project is to predict the impact of wildfires on urban air quality by estimating smoke exposure using historical fire data and AQI. This includes:
1. Calculating annual smoke estimates based on fire proximity and size.
2. Forecasting smoke exposure for the next 25 years using time series models.
3. Analyzing the correlation between wildfire activity and AQI to quantify air quality impacts.

### Key Methods and Approaches

1. **Bounding Box Calculation**:  
   - A bounding box is created around the city of interest to filter fire events within a specific radius. The code calculates latitude and longitude bounds to include fires within 50-mile increments up to a chosen distance threshold (e.g., 650 miles). This approach helps in isolating fires likely to impact the city’s air quality.
   - The bounding box formula used for approximation:
     - `LAT_25MILES = 25.0 * (1.0 / 69.0)`
     - `LON_25MILES = 25.0 * (1.0 / 54.6)`

2. **Data Processing**:
   - **Filtering and Grouping**: The fire and AQI datasets are filtered by year and parameter (e.g., CO, NO2, PM2.5) for yearly aggregation. Data processing includes calculating mean AQI values and normalizing pollutants to create a comprehensive AQI estimate.
   - **Smoke Estimate Calculation**:
 Based on the formula:
    Smoke Estimate = (GIS Acres) / (Distance from City)^2 * Fire Type Weight

where:
  - **GIS Acres** is the area of the fire in acres.
  - **Distance from City** is the distance of the fire from the city in miles.
  - **Fire Type Weight** is a weight assigned based on the type of fire, using a predefined dictionary called `fire_type_weights`. If a specific fire type does not have a weight in the dictionary, a default weight of 0.5 is applied.

  This formula calculates the smoke impact for each fire based on its size and distance from the city, with adjustments for proximity weighting.

3. **Modeling**:
   - **SARIMA**: A Seasonal ARIMA model was used to forecast smoke estimates up to 2050, capturing any cyclical or trend behavior in the data.
   - **Confidence Intervals**: The model provided confidence intervals to communicate prediction uncertainty over the long forecast period.

## Code Explanation

### Data Analysis and Modeling (`part 1 analysis.ipynb`)

1. **Data Import and Processing**:
   - The code begins by importing and cleaning raw datasets from `data/raw/`, including fire data and AQI data. It then calculates yearly averages for AQI parameters and filters fires within the bounding box for smoke estimate calculations.

2. **Visualizations**:
   - A series of plots were created to visualize historical trends and distributions:
     - **Total Acres Burned per Year**: Depicts the annual variation in burned acreage.
     - **Number of Fires by Distance from City**: Shows the distribution of fires within specific distance bands to understand which fires are most likely to impact the city.
     - **Time Series of Smoke Estimates and AQI Estimates**: Compares the smoke estimate trends with AQI to assess their relationship over time.

3. **Forecasting**:
   - **SARIMA Modeling**: Uses a seasonal ARIMA model to forecast smoke estimates for the next 25 years (2025-2050). The model parameters were tuned to fit historical smoke estimate data effectively.
   - **Confidence Intervals**: The model incorporates a 95% confidence interval to show potential variations in the forecast.
   - **SARIMA vs. ARIMA Comparison**: The results of SARIMA were compared against ARIMA, with SARIMA showing better performance in capturing long-term trends.

### Sample Output Files

- **`derived/aqi_estimate_v1.csv`**: Contains the estimated AQI values based on the weighted pollutants.
- **`derived/average_aqi_data_by_year.csv`**: Yearly averages of AQI pollutants for trend analysis.
- **`derived/raw_aqi_data_v7.csv`**: Filtered AQI data used for smoke impact estimates.

### Images

Visualizations saved in `images/` include:
- **`fires_distance_histogram.png`**: Histogram showing fire distribution by distance from the city.
- **`smoke_estimate_forecast_sarima.png`**: SARIMA model forecast for smoke estimates, showing predicted values and confidence intervals.
- **`time_series_fire_smoke_aqi_estimates.png`**: Combined time series of smoke and AQI estimates for correlation analysis.
- **`total_acres_burned.png`**: Yearly total acres burned, highlighting temporal patterns in fire severity.

## Installation and Usage

### Requirements

- Python 3.x
- Required packages: `pandas`, `numpy`, `matplotlib`, `geopy`, `statsmodels`,  `fiona`, `shapely`

### Running the Analysis

Clone the repository:
```
git clone https://github.com/yourusername/wildland-fire-impact-assessment.git
```
- Navigate to the repository directory and open part 1 analysis.ipynb in Jupyter Notebook or Jupyter Lab.
- Run the notebook cells sequentially to reproduce the analysis and visualizations.

### License
This project is licensed under the MIT License. See the LICENSE file for details.
