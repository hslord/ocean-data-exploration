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

## Repository Structure

```
ocean-data-exploration/
├── bolinas-wave-energy-transformation.ipynb
├── fathomnet_squidle_comparative_eda.ipynb
├── /data                    # local data files (gitignored)
└── .gitignore               # ignores .nc, .csv, .mat files
```
