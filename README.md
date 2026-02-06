# ocean-data-exploration 🌊

A repository for exploring and visualizing marine datasets. This project currently focuses on the complex bathymetry and wave energy transformation dynamics of the Northern California coast.

## 📍 Current Focus: Bolinas Bathymetry & Wave Energy
The primary analysis (`bolinas-wave-energy-transformation.ipynb`) examines how wave energy spectra evolve from deep water to the coast at **Bolinas, CA ("The Patch")**. 

The project investigates "Height Retention"—the ratio of nearshore wave height to offshore height to identify how bathymetric features focus or shelter the coastline across different swell periods and directions.

### Key Insights from the Analysis:
* **The W-WNW Window:** Long-period swells from 260-270° experience the highest retention due to bathymetric focusing.
* **The NW Shelter:** Retention drops significantly (approx. 30%) once swell direction moves past 280° toward the North-West.
* **Nonlinear Amplification:** Larger offshore swells (>4m) show disproportionately higher retention at specific western angles compared to smaller swell events.

---

## 💾 Data Requirements
To maintain a lightweight repository, the raw data files are not included. To run the analysis, you must download the following datasets and place them in a `/data` folder:

### 1. Bathymetry Data
* **Source:** NOAA NCEI Bathymetry Data Viewer
* **Required File:** A NetCDF (`.nc`) file covering the Bolinas/Duxbury Reef region.

### 2. Wave Modeling (MOP)
* **Source:** CDIP (Coastal Data Information Program)
* **Required File:** Monitoring and Prediction (MOP) shallow-water hindcasts/nowcasts for the Bolinas nearshore.

### 3. Buoy Observations
* **Source:** CDIP Station 029 (Point Reyes)
* **Data:** Deep-water spectral wave data (Direction, Period, and Significant Wave Height). Pulled directly from the web. 

---

## 🛠 Tech Stack
* **Analysis:** `xarray`, `pandas`, `numpy`
* **Visualization:** `matplotlib`, `seaborn`

## 📂 Repository Structure
* `bolinas-wave-energy-transformation.ipynb`: Main exploratory data analysis and visualization notebook.
* `/.gitignore`: Configured to ignore large `.nc`, `.csv`, and `.mat` files.

## 🚀 Getting Started
1. **Clone the repo:**
   ```bash
   git clone [https://github.com/YOUR_USERNAME/ocean-data-exploration.git](https://github.com/YOUR_USERNAME/ocean-data-exploration.git)
