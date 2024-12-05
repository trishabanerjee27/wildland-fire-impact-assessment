# wildland-fire-impact-assessment

This repository contains code and data for assessing the impact of wildland fires on air quality using historical wildfire data, smoke estimates, and AQI metrics. The project utilizes multiple datasets and predictive models to forecast future smoke exposure and its correlation with AQI (Air Quality Index) over time. The aim is to analyzing the impact of smoke estimates on mortality trends in Syracuse, NY, using integrated datasets from fire and AQI data. The project demonstrates data integration, statistical analysis, visualization, and forecasting techniques.

### Repository Structure
```
wildland-fire-impact-assessment
├── code
│   └── part 1 analysis.ipynb      # Jupyter notebook for data analysis and modeling
│   ├── part_2.ipynb                # Notebook for Part 2: Extension Analysis
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
├── images/
│   ├── correlation_analysis.png           # Correlation matrix visualization
│   ├── deaths_vs_smoke_estimates.png      # Scatter plot of deaths vs smoke
│   ├── fires_distance_histogram.png       # Histogram of fire distances
│   ├── main_deaths_historical_forecast.png# Forecasting mortality trends
│   ├── percentage_of_total_deaths.png     # Percentage trends of death causes
│   ├── smoke_estimate_forecast_sarima.png # Smoke estimates forecast
│   ├── time_series_fire_smoke_aqi_estimates.png # AQI and smoke time series
│   └── total_acres_burned.png             # Visualizing total acres burned
├── .gitignore                     # Git ignore file for managing ignored files
├── LICENSE                        # License file for the project
└── README.md                      # Project README file (you are here)

```

### Folders and Files

- **`code/`**: 
  - **`part_1_analysis.ipynb`**: This notebook covers data loading, preprocessing, modeling, and visualization. It includes steps for calculating smoke estimates, integrating fire and AQI data, performing statistical correlation analyses, and generating visualizations to explore trends and relationships.
  - **`part_2.ipynb`**: This notebook extends the analysis by addressing a human-centered data science question posed during the project. It includes advanced modeling techniques, additional visualizations, and analysis to explore the extended research question. The notebook builds upon Part 1, incorporating new methodologies and data subsets to provide deeper insights into the relationships between smoke exposure, AQI, and mortality trends.
- **`data/derived/`**: Stores cleaned, processed, and derived datasets used for analysis. Files here include various versions of AQI and smoke estimate data, created during different stages of data processing.
- **`data/raw/`**: Contains original datasets like `Fire_Feature_Data.gdb` and `Wildland_Fire_Polygon_Metadata.xml`, which provide wildfire information, and `raw_aqi_data_v6.csv`, a version of the AQI dataset before transformations.
- **`images/`**: Holds visualizations created from the analysis, such as histograms, forecasts, and time series plots that depict fire distance distributions, smoke estimate forecasts, and trends in acres burned.
- **`.gitignore`**: Specifies files and folders to exclude from version control.
- **`LICENSE`**: License for the repository.
- **`README.md`**: This file, providing a detailed overview of the project.

### Data Description for Intermediate Files in `data/derived/`

- **`aqi_estimate_v1.csv`**  
  Contains the first iteration of AQI data estimates derived from the EPA API using Syracuse's Onondaga County as a filter. This file was used to analyze the annual air quality trend but does not cover the full set of parameters across all years. 

- **`average_aqi_data_by_year.csv`**  
  Stores the annual average AQI values for all monitored pollutants, reshaped to present each pollutant as a separate column. Data was processed using the EPA API responses, with missing data interpolated for a smooth analysis of air quality trends.

- **`average_aqi_data_by_year_v2.csv`**  
  An enhanced version of the annual average AQI data using a refined bounding box around Syracuse, NY. This version corrects gaps in the original dataset by expanding the spatial bounds.

- **`average_aqi_data_by_year_v3.csv`**  
  The most complete version of the annual average AQI data. This file includes AQI parameters for all pollutants across the study period by adjusting the bounding box to 100 miles, ensuring more comprehensive coverage.

- **`fire_db_milesfiltered.csv`**  
  Contains geospatial fire data filtered to include only fires within 650 miles of Syracuse, NY, and between 1961 and 2021. This file was processed to calculate annual smoke estimates.

- **`raw_aqi_data.csv`**  
  Raw AQI data fetched from the EPA API for Onondaga County. This file contains pollutant-specific daily AQI values and is the basis for generating yearly averages.

- **`raw_aqi_data_v2.csv`**  
  Raw AQI data using a bounding box of 50 miles around Syracuse, NY. Includes updated parameters but still lacks full coverage for the study period.

- **`raw_aqi_data_v3.csv`**  
  Refined raw AQI data with an expanded bounding box to 100 miles. Addresses gaps identified in previous iterations.

- **`raw_aqi_data_v4.csv`**  
  Final iteration of raw AQI data for bounding box adjustments. This dataset provides the most comprehensive pollutant data and serves as the primary input for calculating air quality metrics.

- **`raw_aqi_data_v5.csv`**  
  Represents the finalized bounding box adjustment for AQI data, containing data across all parameters from 1961 to 2021. This version aggregates the pollutant data across spatial bounds effectively.

- **`yearly_smoke_estimate.csv`**  
  A file summarizing annual smoke exposure estimates based on fire size, distance, and type. This file was generated from `fire_db_milesfiltered.csv` by applying a custom smoke estimation formula.

- **`xml_to_csv.xlsx`**  
  An intermediary file containing converted XML data to CSV for further processing. It might include metadata or initial transformation steps before final aggregation.

These files were processed iteratively to ensure completeness and accuracy, adjusting bounding boxes and filtering criteria to align with the project's objectives.


## Project Overview

### Objectives

The primary goal of this project is to predict the impact of wildfires on urban air quality and health outcomes by estimating smoke exposure using historical fire data, AQI, and mortality rates. This includes:

1. Calculating annual smoke estimates based on fire proximity, size, and type.  
2. Forecasting smoke exposure for the next 25 years using time series models.  
3. Analyzing the correlation between wildfire activity, AQI, and mortality rates to quantify their impacts on public health.  
4. Identifying trends in mortality rates for specific causes of death, such as Chronic Lower Respiratory Disease (CLRD) and heart disease, and assessing their relationship with smoke exposure.
5. Providing actionable insights for public health planning and policy interventions based on the analysis.

### Key Methods and Approaches

1. **Bounding Box Calculation**:  
   - A bounding box is created around the city of interest to filter fire events within a specific radius. The code calculates latitude and longitude bounds to include fires within a distance threshold (e.g., 100 miles). This approach helps in isolating fires likely to impact the city’s air quality.
   - **Bounding Box Formula**:
     - `LAT_25MILES = 25.0 * (1.0 / 69.0)`
     - `LON_25MILES = 25.0 * (1.0 / 54.6)`

2. **Data Processing**:
   - **Filtering and Grouping**: The fire and AQI datasets are filtered by year and parameter (e.g., CO, NO2, PM2.5) for yearly aggregation. Data processing includes calculating mean AQI values and normalizing pollutants to create a comprehensive AQI estimate.
   - **Smoke Estimate Calculation**:
     - Based on the formula:
       ```
       Smoke Estimate = (GIS Acres) / (Distance from City^2 if Distance > 0 else 1) * Fire Type Weight
       ```
       ##### Components of the Formula:
     1. **GIS Acres**:
        - Represents the burned area of the fire in acres.
     
     2. **Distance from City**:
        - Represents the distance of the fire from the city in miles.
        - The formula divides the fire’s burned area by the square of its distance from the city (`Distance^2`) if the distance is greater than 0. For cases where the distance is 0 or missing, the denominator defaults to 1 to avoid division by zero.
     
     3. **Fire Type Weight**:
        - A weight assigned to the fire type using a predefined dictionary (`fire_type_weights`) based on the fire's characteristics.
        - If a specific fire type is not found in the dictionary, a default weight of 0.5 is applied.
     
     This formula adjusts the smoke estimate by scaling the fire’s burned area with its distance from the city, reducing the impact of fires farther away. Additionally, the fire type weight fine-tunes the estimate based on the fire's characteristics.


3. **Modeling**:
   - **SARIMA**: A Seasonal ARIMA model was used to forecast smoke estimates up to 2050, capturing any cyclical or trend behavior in the data.
   - **Confidence Intervals**: The model provided confidence intervals to communicate prediction uncertainty over the long forecast period.

---

## Data Description

### 1. Vital Statistics Deaths by Resident County, Region, and Selected Cause of Death
- **Source**: [New York State Department of Health](http://www.health.ny.gov/statistics/vital_statistics/)
- **Content**:  
  - Death counts by cause, county, and region.  
  - Aggregated annually since 2003.  
- **Metadata**:  
  - **Agency**: New York State Department of Health  
  - **Coverage**: Statewide (New York)  
- **Columns**:  
  - **Year**: Year of death.  
  - **County Name**: County of residence at the time of death.  
  - **Region**: NYC or Rest of State.  
  - **Selected Cause of Death**: Cause classified using ICD-10 codes.  
  - **Deaths**: Count of deaths for the specified year and cause.  
- **Limitations**:  
  - Underreporting for out-of-state resident deaths.  
  - Aggregation biases due to granularity limits.  
- **Use Case**:  
  - Provides mortality data crucial for analyzing trends in health outcomes linked to environmental factors like air quality.  

### 2. Combined Wildland Fire Datasets for the United States and Certain Territories
- **Source**: [U.S. Geological Survey](https://doi.org/10.5066/P9ZXGFY3)  
- **Content**:  
  - Unified fire dataset combining 40 individual sources.  
  - Spanning 1835–2020 with detailed fire polygons.  
- **Metadata**:  
  - **Publisher**: U.S. Geological Survey  
  - **Purpose**: Create a comprehensive fire dataset by merging multiple datasets with varying spatial and temporal scales.  
- **Attributes**:  
  - Fire boundaries for each fire year.  
  - Tier-based quality rankings for data reliability.  
- **Limitations**:  
  - Spatial and temporal resolution differences across source datasets.  
  - Potential overlaps in fire boundary representation.  
- **Use Case**:  
  - Used to estimate smoke impacts based on fire size, proximity, and type, which are linked to air quality and health outcomes.  

---

## Analysis Workflow

### 1. Data Preprocessing
- Filter and clean raw data for usability.  
- Normalize fire and mortality datasets to ensure compatibility.  

### 2. Smoke Estimate Calculation
- Formulate smoke estimates considering:
  - **Fire size (GIS_Acres)**.  
  - **Proximity to the city (dist_from_city)**.  
  - **Fire type weights** (e.g., wildfires, prescribed burns).  

### 3. Integration
- Merge mortality statistics with smoke estimates to analyze relationships.  

### 4. Statistical Analysis
- Use Pearson correlation to measure relationships between smoke estimates and causes of death.  

### 5. Visualization
- Generate charts for trends, correlations, and forecasts.  

### 6. Forecasting
- Employ SARIMA models to predict future mortality trends.  

---

## Code Explanation

### Data Analysis and Modeling (`part_1_analysis.ipynb`)

1. **Data Import and Processing**:
   - Imports and cleans raw datasets from `data/raw/`.
   - Calculates yearly averages for AQI parameters.
   - Filters fires within the bounding box for smoke estimate calculations.

2. **Visualizations**:
   - **Total Acres Burned per Year**: Depicts annual variations in burned acreage.
   - **Number of Fires by Distance from City**: Shows distribution of fires likely to impact the city.
   - **Time Series of Smoke Estimates and AQI Estimates**: Compares smoke and AQI trends.

3. **Forecasting**:
   - **SARIMA Modeling**: Forecasts smoke estimates for 2025–2050.
   - **Confidence Intervals**: Includes uncertainty bounds for predictions.

---

### Sample Output Files

- **`derived/aqi_estimate_v1.csv`**: Contains estimated AQI values.
- **`derived/average_aqi_data_by_year.csv`**: Yearly averages of AQI pollutants.
- **`derived/raw_aqi_data_v7.csv`**: Filtered AQI data for smoke estimates.

---

### Images

- **`deaths_vs_smoke_estimates.png`**: Scatter plot showing the relationship between smoke estimates and deaths caused by respiratory and cardiovascular conditions. This visualization highlights potential correlations between smoke exposure and mortality rates.
- **`fires_distance_histogram.png`**: Histogram displaying the distribution of fires by their distance from the city, illustrating which fires are most likely to impact the city's air quality.
- **`main_deaths_historical_forecast.png`**: Visualization of historical mortality trends and forecasted deaths for selected causes (e.g., CLRD and heart disease). Includes confidence intervals for future projections.
- **`percentage_of_total_deaths.png`**: Line plot showing the percentage trends of different causes of death over time, emphasizing shifts in mortality patterns.
- **`smoke_estimate_forecast_sarima.png`**: Forecast of annual smoke estimates using the SARIMA model. Includes confidence intervals to demonstrate the range of possible future values.
- **`time_series_fire_smoke_aqi_estimates.png`**: Time series plot comparing smoke estimates with AQI trends, providing insights into how wildfire activity correlates with air quality changes.
- **`total_acres_burned.png`**: Bar chart visualizing the total acres burned annually, highlighting patterns and variability in wildfire activity over the years.


## Instructions for Reproducibility

1. **Clone the repository**:
   ```
   git clone https://github.com/trishabanerjee27/wildland-fire-impact-assessment.git
      ```
2. **Requirements**:

- **Python**: Python 3.x is required for running the analysis and scripts.
- **Required Packages**:
  - `pandas==2.2.3`
  - `numpy==2.1.3`
  - `matplotlib==3.8.3`
  - `geopy==2.4.1`
  - `statsmodels==0.14.4`
  - `fiona==1.10.1`
  - `shapely==2.0.6`
  - `folium==0.18.0`
  - `geopandas==1.0.1`
  - `Requests==2.32.3`
  - `scikit_learn==1.4.1.post1`
  - `scipy==1.14.1`
  - `seaborn==0.13.2`

To install these dependencies, use the provided `requirements.txt` file:
```bash
pip install -r requirements.txt
```
3. Run notebooks in ```code/ ``` for analysis and visualization.


### License
This project is licensed under the MIT License. See the LICENSE file for details.

## Acknowledgments

### Data Sources
- [New York State Vital Statistics](http://www.health.ny.gov/statistics/vital_statistics/)
- [USGS Combined Wildland Fire Dataset](https://doi.org/10.5066/P9ZXGFY3)
- [EPA AQS Data Mart](https://aqs.epa.gov/data/api)

### Contributors
- **Trisha Banerjee**

For further inquiries, contact [tbaner@uw.edu](mailto:tbaner@uw.edu).
