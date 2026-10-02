# Critical Minerals and Environmental Conflict in Latin America

**TeamTokio — Week 3 Project**

Rising global demand for critical minerals (lithium, copper, nickel, graphite…) is pushing mining into new areas of Latin America. This project asks whether that expansion goes hand in hand with more environmental conflict and more violence against the people who defend land and water.

> **Research question:** Do resource-rich countries in Latin America see more killings of environmental defenders?

We test this by bringing together data on mining activity, mineral demand, mining-related conflicts and killings of defenders, and looking for **hotspots** where they overlap.

---

## Team

| Part | Data source | Owner |
|---|---|---|
| Killings of environmental defenders | Global Witness | Diego |
| Conflict events | mapa.conflictosmineros.net | Diego |
| Critical-mineral supply and demand | IEA Critical Minerals Dataset 2026 | Lena |
| Large-scale mining sites | ICMM Global Mining Dataset v1.5 | Lena |

---

## Data sources

| # | Source | What it measures | Coverage |
|---|---|---|---|
| 1 | **Global Witness** – land and environmental defenders killed | Main measure of violence | 1,722 killings in Latin America, 2012–2025 (200 linked to mining & extractives) |
| 2 | **IEA** – Global Critical Minerals Outlook / Critical Minerals Dataset 2026 | Current and projected mining supply; global demand trend | Supply for Argentina, Brazil, Chile, Peru, 2025–2040; demand at world level only, 2025–2050 |
| 3 | **ICMM Global Mining Dataset v1.5** (July 2026, CC BY 4.0) | Where mines, smelters and refineries are — our measure of "resource-rich" | 709 sites in 20 Latin American and Caribbean countries (2026 snapshot) |
| 4 | **Mining conflicts** (titles match OCMAL – Observatorio de Conflictos Mineros de América Latina) | Mining-related social/environmental conflicts | 284 conflicts in 26 countries, start years 1972–2019 |
| 5 | **mapa.conflictosmineros.net** | Conflict events and their geography related to mining activities | Via webscrapping |

**"Critical mineral"** in this project means a mineral listed in the IEA Critical Minerals Dataset 2026. Tin, antimony, iron ore, bauxite and coal are therefore *not* flagged as critical.

---

## Repository layout

```
├── notebooks/        Cleaning, visualisation and analysis notebooks
├── clean/            Cleaned outputs written by the notebooks (CSV + XLSX)
├── data/             Copies of key cleaned tables
├── figures/          Charts and maps (PNG, HTML)
└── README.md
```

### Notebooks

Each notebook works on **its own dataset only**. Combining datasets is a separate, later step.

| Notebook | Purpose | Main outputs |
|---|---|---|
| `IEA_data_cleaning_nV2.ipynb` | Clean IEA supply data (mining only, 12 South American countries, 2025–2040) | `iea_data_clean.csv` (288 rows) |
| `iea_data_visualisation.ipynb` | Seven charts on supply trends, % change, extrapolation to 2050, country shares | `figures/` |
| `icmm_mining_sites_cleaning.ipynb` | Clean ICMM sites: encoding fixes, mineral vocabulary, confidence, duplicate flags | `icmm_sites_latam.csv` (709 × 33), `icmm_site_commodities.csv` (1,616), `icmm_mining_sites_clean.xlsx` |
| `icmm_mining_sites_visualisation.ipynb` | Site map, sites per country, gold vs critical, country × mineral heatmap | `figures/` |
| `mining_conflicts_cleaning.ipynb` | Clean conflict list: country names/ISO3, start years, keyword flags | `mining_conflicts_latam.csv`, `mining_conflicts_country.csv`, `mining_conflicts_clean.xlsx` |
| `mining_conflicts_visualisation.ipynb` | Conflicts by country, region and start period | `figures/` |
| `mining_conflicts_map_layers.ipynb` | Layered maps: ICMM sites + conflict provinces (plotly, folium, static) | `clean/conflict_zones.csv`, `clean/admin1_summary.csv` |
| `defenders_vs_conflicts.ipynb` | Compare Global Witness killings with mining conflicts by country | `clean/defenders_vs_conflicts_*.csv`, 5 PNGs |
| `defenders_conflicts_maps.ipynb` | Side-by-side, overlay and "gap" maps of killings vs conflicts | `figures/map_*.html` |

---

## Getting started

```bash
git clone https://github.com/len-chou/TeamTokio---Week-3-Project-.git
cd TeamTokio---Week-3-Project-
pip install pandas numpy matplotlib seaborn plotly folium openpyxl scipy jupyter
jupyter notebook
```

The notebooks read their input data **directly from this GitHub repo** (raw links), so they run without local copies. Run a cleaning notebook before its matching visualisation notebook.

The layered-map notebook needs `latam_countries.geojson` and `latam_admin1.geojson` (Natural Earth, simplified) next to it or in the repo; otherwise it downloads Natural Earth (~45 MB) and simplifies it.

---

## Key findings so far

**Mineral supply (IEA)**
- Copper supply falls from 8,211 to 6,024 kt between 2025 and 2040 (Chile −24 %, Peru −32 %, mostly after 2030).
- Lithium rises from 70.8 to 113 kt; Argentina grows +191 % (18.7 → 54.4 kt, almost all by 2030) and matches Chile from 2030.
- Brazil graphite +54 % by 2030. **Expansion candidates:** Argentina (lithium), Brazil (graphite).

**Mining sites (ICMM)**
- Brazil 196, Peru 129, Mexico 109, Chile 102 sites — 536 of 709.
- 92 % of sites are mines; 27 of 45 smelters/refineries are in Brazil. Only 11 sites have lithium as main mineral.

**Mining conflicts**
- Mexico 58, Chile 49, Peru 46, Argentina 28, Brazil 26, Colombia 19 (78 % of rows). Peak start period 2005–2009.
- 78 % of placed conflicts are in provinces that also have a large mining site. 68 % of provinces with a large site had a conflict, vs 14 % without (association, not cause).

**Defenders vs conflicts**
- Weak link: Spearman ρ = 0.35 (p = 0.19) on the 16 countries in both datasets; 0.52 (p = 0.02) including zeros.
- Agreement in Mexico, Peru and Brazil. Big mismatches: Chile (49 conflicts, 0 mining killings), Argentina (28, 0), Colombia (19 conflicts, 47 mining killings).
- In mining cases, police and armed forces appear more often as perpetrators than in other cases.

---

## Limitations

- **Scope:** IEA data covers South America only (and names only 4 countries; others are "not reported", not zero). Mexico and Central America are not yet in the supply analysis. Gold is not in the IEA data.
- **Time:** ICMM is a single 2026 snapshot; conflicts stop in 2019; Global Witness starts in 2012 — overlap is only 2012–2019.
- **Counts ≠ production:** ICMM gives site counts, not output. 26 % of Latin American sites have "Very Low" confidence (69 % in Bolivia).
- **Geography:** killings have no coordinates; country maps use one point per country.
- **Conflict data:** the source still needs confirming (appears to be OCMAL); coverage may be uneven by country; keyword flags come from titles only.

## Next steps

- Combine datasets at country and province level (ICMM sites, IEA supply, conflicts, killings, ACLED events).
- Add population and production to compute per-capita and per-production rates.
- Extend coverage to Mexico and Central America.

---

## Archived / unused data

These files are kept for reference but are **not used** in the analysis:

- `latam_critical_minerals_crime_4countries.csv` / `.xlsx` — 188 crime, enforcement and violence records for Argentina, Brazil, Chile and Peru (Chile SMA/SNIFA sanctions register 124, Global Witness 40, documented cases 24). The `.xlsx` adds Summary, Codebook and "Sources & gaps" sheets.
- `latam_critical_minerals_env_crime.csv`, `crime_latam_clean.csv` — earlier crime files.
- Amazon Mining Watch gold detector (`amw_*.csv`, `gold_mining_detector_cleaning.ipynb`) and Colombia EVOA (`col_evoa_department_year.csv`) — dropped.
- USGS mineral exploration sites (2016) — dropped, superseded by ICMM.
- Earlier versions: `icmm_sites_latam_clean.csv`, `country_resource_profile.csv`, `iea_supply_vs_energy_demand.csv`, `iea_minerals_combined_long.csv`.

---

## Data licences and credits

- ICMM Global Mining Dataset v1.5 — CC BY 4.0, International Council on Mining and Metals.
- IEA Critical Minerals Dataset 2026 — © IEA; check IEA terms before redistribution.
- Global Witness defenders data — Global Witness.
- ACLED — Armed Conflict Location & Event Data Project; subject to ACLED terms of use.
- Mining conflicts — OCMAL (to be confirmed).
