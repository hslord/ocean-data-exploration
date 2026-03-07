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
- SQUIDLE+ is ~23× larger by image count; FathomNet is ~4× denser in annotations per image.
- FathomNet is ROV-only (sole registered imaging type); SQUIDLE+ spans all 6 major collection types.
- SQUIDLE+ annotation taxonomy: Physical substrate annotations outnumber biological at the root level and beyond.
- The two databases serve complementary niches: FathomNet for ROV/deep-water species detection; SQUIDLE+ for AUV, diver, towed, and benthic survey work.

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
