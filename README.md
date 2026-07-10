# ocean-data-exploration

A repository for exploring and visualizing marine datasets — spanning physical oceanography, bathymetry, and underwater imagery.

---

## Notebooks

### 1. Bolinas Bathymetry & Wave Energy Transformation
**File:** `bolinas-wave-energy-transformation.ipynb`

Examines how wave energy spectra evolve from deep water to the coast at **Bolinas, CA ("The Patch")**. The analysis investigates Height Retention — the ratio of nearshore wave height to offshore height — to identify how bathymetric features focus or shelter the coastline across different swell periods and directions.

**Key Findings:**
- **The W-WNW Window:** Long-period swells from 260–270° experience the highest retention due to bathymetric focusing.
- **The NW Shelter:** Retention drops significantly (~30%) once swell direction moves past 280° toward the NW.
- **Nonlinear Amplification:** Larger offshore swells (>4m) show disproportionately higher retention at specific western angles.

**Data Requirements** (not included; place in `/data`):
- **Bathymetry:** NetCDF (`.nc`) from NOAA NCEI Bathymetry Data Viewer covering the Bolinas/Duxbury Reef region.
- **Wave Modeling:** MOP shallow-water hindcasts/nowcasts from CDIP (Coastal Data Information Program).
- **Buoy Observations:** CDIP Station 029 (Point Reyes) — pulled directly from the web; no download needed.

**Dependencies:** `xarray`, `pandas`, `numpy`, `matplotlib`, `seaborn`

---

### 2. FathomNet & SQUIDLE+ Comparative EDA
**File:** `fathomnet_squidle_comparative_eda.ipynb`

A comparative exploratory data analysis of two major underwater image annotation databases to inform collection strategy for marine computer vision projects. Analysis is parameterised by collection type (AUV, ROV, Diver, Towed, BRUVs, Drop Camera) as the primary decision axis.

- **FathomNet** — MBARI-managed; ~448K images with bounding-box annotations; ROV-only platform; NE Pacific bias; deep-sea focus.
- **SQUIDLE+** — IMOS/ACFR-managed; ~10.5M images with point-label annotations (CATAMI scheme); 6 collection types; global coverage.

**Sections:**
1. Scale & density comparison
2. Platform & sensor profile
3. Geographic, depth & temporal coverage
4. Annotation landscape (label taxonomy at lineage levels 0–2)
5. Image sample visualization with annotations
6. Deployment synthesis — database selection by collection type

**Key Findings:**
- **Scale**: SQUIDLE+ is ~23× larger (10.5M vs 448K images), but FathomNet is ~4× denser (2.41 vs 0.61 annotations/image); SQUIDLE+ spans 25,441 deployments across 32 platforms.
- **Platform lock-in**: FathomNet is ROV-only — 9 institutions, with the top 3 (NOAA NMFS SEFSC, MBARI, NOAA Ocean Exploration) accounting for 91% of all images. SQUIDLE+ explicitly tracks collection type for every deployment: AUV (4 platforms), BRUVs (8), Diver (5), Drop Camera (3), ROV (6), Towed (5).
- **Depth split**: FathomNet is a deep-water database (median ~866 m, range 3–5,831 m; NE Pacific bias). SQUIDLE+ is predominantly shallow — 94% of deployments are under 100 m, with global geographic reach (43/72 grid cells populated).
- **Label format and class imbalance**: FathomNet uses bounding boxes across 2,870 taxa, but the distribution is severely long-tailed — 117 taxa account for 80% of all boxes, and the single top label (*Lutjanus campechanus*) holds 14% of all annotations. SQUIDLE+ uses CATAMI point labels dominated by physical substrate: L0 "Physical" leads with 1.23M annotations, L1 "Substrate" with 1.2M, and L2 "Unconsolidated (soft)" with 1.06M. Critically, CATAMI is not a single hierarchy — it defines parallel classification axes (biota identity, physical substrate, geoform, ecological status), so the same real-world object can be labelled under multiple axes simultaneously. Keyword filtering alone is unreliable; axis selection is a prerequisite before building any training set from SQUIDLE+.
- **Complementary niches**: FathomNet is the right source for ROV/deep-water species detection (YOLO-ready boxes). SQUIDLE+ covers AUV, diver, towed, BRUV, and benthic habitat work (CATAMI substrate classification). Mixing them without domain alignment is likely to hurt model performance.

**Dependencies:** `fathomnet` (PyPI), `pandas`, `numpy`, `matplotlib`, `Pillow`, `requests`

Both databases are accessed live — FathomNet via the `fathomnet` Python library; SQUIDLE+ via the public REST API (`https://squidle.org/api`) — no downloads required.

---

### 3. Kelp Ecosystem EDA — CMEMS Oceanographic Pipeline
**File:** `Copernicus_Marine_Kelp_Ecosystem_EDA.ipynb`

Exploratory data analysis of four Copernicus Marine (CMEMS) satellite and reanalysis products for the **California kelp coast (Monterey → Mendocino, 36.5–40.0°N)**, spanning 2002 to present. Each product is constructed as a **MY (reprocessed) + NRT/NFC (operational) blend** for full historical and near-real-time coverage. Section 0 validates every product seam before analysis proceeds.

**Data Sources (all via CMEMS):**

| Product | Variables | Date range | Spatial res. |
|---|---|---|---|
| SST — OSTIA L4 | `analysed_sst` (skin °C) | 2002–present | 0.05° (~5 km) |
| Chl-a — OC-CCI L4 multi-sensor | `CHL` (mg m⁻³) | 2002–present | 4 km |
| Subsurface T/S — GLORYS12 | `thetao`, `so` (0–30 m) | 2002–present | 0.083° |
| SSH & currents — DUACS | `adt`, `ugos`, `vgos` | 2002–present | 0.25° |

**Sections:**
1. **Seam validation** — MY vs NRT/NFC bias check; ADT bias-corrected (+0.013 m)
2. **SST** — monthly time series and year × stage heatmap
3. **Chlorophyll-a** — anomalously low stage flags, nearshore vs offshore gradient
4. **Subsurface T/S** — thetao/salinity heatmaps, nearshore freshening, SST vs thetao comparison
5. **SSH & geostrophic currents** — ADT heatmap, peak-stage composite vector map
6. **Correlation matrix & phase transitions** — cross-variable correlations, low-chl-a year identification
7. **Anomaly analysis** — monthly anomalies from a 2002–2012 pre-collapse baseline
8. **Spatial decomposition** — latitude-binned anomalies (Monterey, Bodega/SF, Mendocino) and record-SST map
9. **Lead-lag cross-correlations** — Pearson CCF at ±6 months
10. **Thermocline depth** — depth of max |dT/dz| in GLORYS 0–30 m column
11. **Stress-year composites** — anomaly comparison, stress years (2014, 2022–2025) vs neutral
12. **Trend analysis** — per-stage OLS linear trends for all five variables

**Key Findings:**
- **2014 marine heatwave** dominates the record: SST anomaly +4.12°C (Sep 2014), subsurface thetao +3.32°C simultaneously — the anomaly penetrated the full 0–30 m layer, not skin-only.
- **ADT → chl-a** is the strongest ecological coupling (r = −0.55): elevated SSH suppresses upwelling and surface nutrients.
- **Stress-year signature**: five stress years (2014, 2022–2025) each show their largest departure from neutral during **Senescence (Sep–Nov)** across all five variables — SST +1.25°C, thetao +1.42°C, chl-a −0.20 log1p, ADT +0.07 m, salinity −0.14 PSU.
- **2025** is the only year on record with three consecutive productive stages (Recruit, Growth, Peak) simultaneously below within-stage thresholds.
- **Significant trends**: ADT rising in all stages (p < 0.005); subsurface thetao warming in Winter (p = 0.022); Chl-a declining during Growth (p = 0.012, −0.009 log1p/yr); SST surface trends not significant in any stage.
- **Nearshore chl-a 2.19× higher** than offshore; 21 estuarine/inland-water pixels excluded via time-mean mask.
- **Thermocline depth**: mean 12.7 m ± 9.4 m; shallowest in late summer (upwelling-driven), 32 of 296 months fully mixed.

**Dependencies:** `copernicusmarine`, `xarray`, `numpy`, `pandas`, `matplotlib`, `scipy`

Data is fetched directly from CMEMS via the `copernicusmarine` Python client. A free CMEMS account is required.

---

### 4. Kelp Canopy EDA — KelpWatch Landsat Time Series
**File:** `kelp_canopy_eda.ipynb`

Exploratory data analysis of the SBC LTER / KelpWatch Landsat-derived kelp canopy dataset for the **California kelp coast (Monterey → Mendocino, 36.5–40.0°N)**, 2002–present. Quarterly 30 m canopy area and giant kelp biomass across 45,564 coastal pixels. Analysis window matches the Copernicus Marine notebook to enable future joint analysis.

**Sections:**
1. **Data quality** — sensor timeline, SLC failure, coverage gaps
2. **Canopy area time series** — domain mean and total extent, year × quarter heatmap
3. **Spatial decomposition** — latitude-binned time series and heatmaps (Monterey / Bodega-SF / Mendocino) as species proxy
4. **Anomaly analysis** — quarterly anomalies from 2002–2012 pre-collapse baseline
5. **Pre vs post-collapse composites** — per-quarter mean area, pre (2002–2012) vs post (2013–present)
6. **Trend analysis** — full-record OLS, Mann-Whitney pre/post break, post-collapse OLS (Monterey), biomass vs canopy area divergence
7. **Satellite basemap overlay** — KelpWatch pixels on USGS imagery, best vs worst Q3 comparison
8. **Summary of findings**

**Key Findings:**
- **Collapse onset at 2013** is visible as a sharp transition across all time series and heatmaps; best Q3 on record is 2012; worst is 2023 
- **Q3 Peak:** domain mean fell from 87.2 m²/pixel (pre-collapse) to 30.1 m²/pixel (post-collapse) — a 65% loss
- **Two structurally different collapses by species:** giant kelp (*Macrocystis*, Monterey) suffered a step-change and continues to erode post-collapse — Q3 losing ~6 m²/pixel per quarter; bull kelp (*Nereocystis*, Mendocino) collapsed abruptly with the cliff extending into Q2 (spring recruitment), and shows no further detectable trend since 2013
- **Decline mode — canopy thinning:** Monterey biomass falls faster than canopy area post-collapse; the geographic footprint of patches persists but canopy density is declining

**Data Requirements:** Single ~2.4 GB NetCDF file (SBC LTER KelpWatch, `knb-lter-sbc.74.29`) — downloaded automatically via the EDI PASTA public API. No login required. Cached to `/data/kelpwatch_cache/` (gitignored).

**Dependencies:** `xarray`, `numpy`, `pandas`, `matplotlib`, `scipy`, `requests`, `tqdm`, `netCDF4`, `Pillow`

---

### 5. Kelp Ecosystem — Species Observation EDA
**File:** `kelp_species_observation_eda.ipynb`

Multi-source presence/abundance analysis of four priority species on the **California kelp coast (Monterey → Mendocino, 36.5–40.0°N)**, 2002–present. Four data sources are layered from most quantitative (BCO-DMO ground truth) to broadest institutional coverage (OBIS + GBIF targeted datasets) to highest citizen science volume (iNaturalist). Produces a unified `species_obs_combined.parquet` artifact for downstream ML labeling.

| Source | API / Library | Data type |
|---|---|---|
| BCO-DMO ERDDAP (5 datasets) | `erddapy` → `erddap.bco-dmo.org/erddap` | Abundance / density (ground truth) |
| OBIS | `pyobis` | Presence / absence — institutional records |
| GBIF targeted datasets | `requests` → `api.gbif.org` | Monitoring + occurrence (MLPA + Bull Kelp DB) |
| iNaturalist | `pyinaturalist` | Presence / absence — citizen science (research grade) |

**Priority species:** *Strongylocentrotus purpuratus* (purple urchin), *Pycnopodia helianthoides* (sunflower sea star), *Nereocystis luetkeana* (bull kelp), *Macrocystis pyrifera* (giant kelp)

**Sections:**
1. **ETL** — BCO-DMO (5 datasets), OBIS, iNaturalist, GBIF (MLPA + Bull Kelp DB by `datasetKey`); unified `species_obs_combined.parquet` output
2. **BCO-DMO ground truth** — urchin density, community surveys (1999–2016), recruitment
3. **OBIS** — annual observation counts and spatial distribution by species
4. **iNaturalist** — annual trends, sunflower sea star effort-normalised trend, observer effort and spatial bias
5. **GBIF targeted datasets** — MLPA monitoring archive + Bull Kelp Database; recency and coverage analysis
6. **Cross-source synthesis** — combined annual timeline (all 7 sources), coverage summary table, records by data layer
7. **Summary of findings**

**BCO-DMO datasets:** `927682_v1` (MLPA/PISCO invertebrate counts), `928527_v1` (substrate), `661081` (Monterey Bay community surveys 1999–2016), `661151` (percent cover), `929812_v1` (urchin recruitment)

**Data Requirements:** All data fetched live — no downloads required. Cached to `/data/species_obs_cache/` (gitignored).

**Dependencies:** `erddapy`, `pyobis`, `pyinaturalist`, `requests`, `numpy`, `pandas`, `matplotlib`

---

### 6. Monterey Kelp Canopy — Sensor Design Analysis
**File:** `monterey_kelp_canopy_analysis.ipynb`

Exploratory analysis of oceanographic variables inside vs. outside giant kelp canopy at **Hopkins Marine Station, Monterey (~36.6°N)**, using co-located mooring data from BCO-DMO. Goal: identify which variables are most diagnostic of kelp presence for a fixed mooring deployment.

**Dataset:** BCO-DMO 822549 — 9-depth paired mooring, `KELP` (inside canopy) vs. `OUTSIDE` (~115m offshore), Jun–Oct 2018. Variables: pH, DO, temperature, salinity, PAR. Analysis follows Hirsh et al. (2020), *JGR Oceans*.

**Sections:**
1. **Data profile** — sensor depth map, variable coverage, MAB geometry
2. **Temperature** — matched-depth profiles and stratification (surface − ~9m)
3. **Diurnal pH and DO** — day/night split at sub-canopy depths; tests for kelp metabolic signal
4. **pH, DO, and salinity time series** — inside-vs-outside differences and spike asymmetry
5. **Sensor recommendations** — variables ranked by kelp-presence signal strength

**Key Findings:**
- **PAR is the only unambiguous canopy indicator.** The KELP bottom sensor (9.9m) records a daytime mean of 4.60 μmol m⁻² s⁻¹ vs. 11.23 μmol m⁻² s⁻¹ at OUTSIDE (17.7m) — 59% lower *despite* being 7.8m shallower. This inversion of expected depth-attenuation is a direct canopy signature.
- **pH and DO are not diagnostic of kelp presence** at the depths sampled. Both are lower inside the kelp at sub-canopy depths (pH −0.04 to −0.19; DO −22 to −32 μmol/kg), driven by canopy shading and physical advection rather than kelp metabolism. The surface layer where photosynthetic buffering occurs (~1–5m) is uninstrumented in the moored record.
- **Temperature shows no kelp metabolic signal** (<0.2°C at all matched depths; stratification comparable between sites). Salinity differences (<0.06 PSU) are confounded by sensor geometry and cannot be attributed to kelp.
- **Internal tidal bores** drive semidiurnal variability at both sites; OUTSIDE is more variable short-term, but seasonal means are lower inside for DO and pH.
- **Recommended sensor priority for a kelp-bed mooring:** PAR (~5–10m) → pH (depth-dependent) → DO → CTD.

**Data Requirements:** `mooring_inside_outside.csv` from BCO-DMO dataset 822549 — downloaded manually from [bco-dmo.org/dataset/822549](https://www.bco-dmo.org/dataset/822549); place in `data/bco_dmo/`.

**Dependencies:** `pandas`, `numpy`, `matplotlib`

---

## Repository Structure

```
ocean-data-exploration/
├── bolinas-wave-energy-transformation.ipynb
├── fathomnet_squidle_comparative_eda.ipynb
├── Copernicus_Marine_Kelp_Ecosystem_EDA.ipynb
├── kelp_canopy_eda.ipynb
├── kelp_species_observation_eda.ipynb
├── monterey_kelp_canopy_analysis.ipynb
├── /data                    # local data files (gitignored)
└── .gitignore               # ignores .nc, .csv, .mat files
```
