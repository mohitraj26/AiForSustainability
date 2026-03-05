# 🌍 Delhi Airshed Land-Use Classification using Sentinel-2 & ESA WorldCover

## 📌 Project Overview

This project implements a complete **geospatial + deep learning pipeline** to analyze land-use patterns in the Delhi-NCR Airshed using:

- Sentinel-2 RGB satellite image patches (128×128 pixels, 10m resolution)
- ESA WorldCover 2021 land cover raster (10m resolution)
- Delhi-NCR & Delhi-Airshed shapefiles (EPSG:4326)

The objective is to:
- Perform spatial filtering of satellite images  
- Generate land-cover labels from raster data  
- Train a CNN model for supervised land-use classification  
- Evaluate model performance using accuracy, F1-score, and confusion matrix  

---

# 🗂 Dataset Description

| Dataset | Description |
|----------|-------------|
| Delhi-NCR Shapefile | Region boundary (EPSG:4326) |
| Delhi-Airshed Shapefile | Airshed boundary (EPSG:4326) |
| Sentinel-2 Image Patches | 128×128 RGB images (10m/pixel resolution) |
| ESA WorldCover 2021 (`land_cover.tif`) | Land-cover raster (10m resolution) |

CRS used for gridding: **EPSG:32644 (UTM Zone 44N)**

---

# 🧭 Q1: Spatial Reasoning & Data Filtering

## 1️⃣ Delhi-NCR Plot with 60×60 km Grid

- Reprojected shapefile to EPSG:32644  
- Generated uniform 60km × 60km grid  
- Visualized using `matplotlib`  
- Basemap added using `geemap`

## 2️⃣ Satellite Image Filtering

- Extracted image center coordinates from filenames  
- Converted to GeoDataFrame  
- Filtered images whose center falls inside Delhi-NCR boundary  

---

# 🏷 Q2: Label Construction & Dataset Preparation

## 1️⃣ Extract Land-Cover Patch

For each satellite image:

- Converted center coordinate → raster CRS  
- Extracted corresponding 128×128 window from `land_cover.tif`  
- Ensured correct spatial alignment at 10m resolution  

## 2️⃣ Label Assignment (Mode-based)

Each image label is determined by dominant class in the extracted patch.

Example:

| ESA Class Code | Pixel Count |
|---------------|------------|
| 50 (Built-up) | 8000 |
| 40 (Cropland) | 3000 |
| 10 (Tree Cover) | 512 |

Assigned label → **Built-up (50)**

---

## 3️⃣ ESA Class Mapping

| ESA Code | Simplified Category |
|----------|--------------------|
| 10 | Vegetation |
| 20 | Vegetation |
| 30 | Vegetation |
| 40 | Cropland |
| 50 | Built-up |
| 80 | Water |
| Others | Others |

Final classes used for training:
- Built-up  
- Vegetation  
- Cropland  
- Water  
- Others  

---

## 4️⃣ Train-Test Split

- 60% Training  
- 40% Testing  
- Random split using `sklearn.model_selection.train_test_split`  
- Visualized class distribution using bar plots  

---

# 🤖 Q3: Model Training & Evaluation

## Model Used

- ResNet18 (Transfer Learning)
- Epochs: 10  


## 📈 Evaluation Metrics

| Metric | Value |
|--------|--------|
| Accuracy | 72.4% |
| F1-Score | 0.715 |

---

## 📊 Confusion Matrix

- Generated using `sklearn.metrics.confusion_matrix`
- Visualized using matplotlib/seaborn

### Interpretation

- High performance observed for dominant classes (e.g., Built-up)  
- Some confusion between Vegetation and Cropland due to spectral similarity  
- Class imbalance impacts minority categories  

---

# 🛠 Tech Stack

- Python  
- GeoPandas  
- Rasterio  
- Shapely  
- Matplotlib  
- Geemap  
- PyTorch  
- Scikit-learn  

---



# 🚀 How to Run

## 1️⃣ Clone Repository
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name


## 2️⃣ Install Dependencies
pip install -r requirements.txt


## 3️⃣ Run Notebook
jupyter notebook notebooks/main-py.ipynb


---

# 🧠 Key Learnings

- CRS transformation and spatial filtering  
- Raster window extraction using geographic coordinates  
- Mode-based label generation from categorical rasters  
- End-to-end geospatial + deep learning pipeline  
- Model evaluation using F1-score and confusion matrix  

