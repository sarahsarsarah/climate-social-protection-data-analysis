# A Data Analysis of Climate-Responsive Social Protection

**Who has protection when a climate shock hits and who doesn't?**

This project explores the global gap between climate vulnerability and social protection coverage across 192 countries. It was built as the final project for the [Ironhack Data Analysis Bootcamp](https://www.ironhack.com), March 2026.

**Author:** Sarah El Jamal  
**Tools:** Python · Pandas · Matplotlib · Plotly · GeoPandas · Scipy · Tableau Public  
**Data:** 5 public international datasets · 192 countries · 30 variables

---

## The Research Question

When a climate shock hits, who has social protection, and who doesn't?

Climate-responsive social protection (CRSP) refers to social protection systems specifically designed to account for climate risk, not just general hardship, but the loss that happens when a drought wipes out a harvest or a flood destroys a livelihood. This analysis asks whether the countries most exposed to climate risk are the ones with systems designed to handle it.

---

## Repository Structure

```
├── notebooks/
│   ├── 01_data_loading.ipynb          # Data import, cleaning, country name harmonisation
│   ├── 02_index_construction.ipynb    # CLVI and Protection Gap Score construction
│   ├── 03_hypothesis_testing.ipynb    # Statistical tests for H1–H5
│   ├── 04_case_studies.ipynb          # Deep dives: Mozambique, Somalia, Haiti, CAR
│   ├── 05_visualisations.ipynb        # All 6 charts (Matplotlib + Plotly)
│   └── 06_geopandas_maps.ipynb        # Geographic visualisations using GeoPandas
│
├── data/
│   └── raw/                           # Source datasets (see Data Sources below)
│                                      # Natural Earth shapefile downloaded automatically on first run
│
├── outputs/
│   ├── master.csv                     # Merged master dataset
│   ├── master_indexed.csv             # With CLVI, SP score, protection gap
│   ├── hypothesis_summary.csv         # H1–H5 results
│   ├── case_studies_table.csv         # Case study profiles
│   ├── ilo_aggregated.csv             # Aggregated ILO data (Tableau-ready)
│   ├── charts/                        # Plotly and Matplotlib chart outputs
│   └── geopandas/                     # GeoPandas map outputs (6 maps)
│
└── README.md
```

---

## Data Sources

| Dataset | Source | Coverage | Variables Used |
|---|---|---|---|
| ND-GAIN Country Index | Notre Dame Global Adaptation Initiative | 192 countries | Climate vulnerability & readiness scores |
| World Risk Index (WRI) | Bündnis Entwicklung Hilft | 193 countries | Physical hazard exposure, coping & adaptive capacity |
| ILOSTAT — SDG 1.3.1 | International Labour Organization | 36,000+ observations | Social protection coverage rates by benefit type |
| World Development Indicators | World Bank | 17,000+ observations | GDP per capita, poverty ratio, life expectancy, health expenditure |
| WIID / WID | UNU-WIDER / World Inequality Database | 11,800+ observations | Gini coefficient, Palma ratio, income shares |

**Note:** Two intended datasets, ASPIRE (benefit adequacy) and UNICEF adaptive SP mechanisms, were unavailable as clean public datasets and are flagged as data gaps throughout the analysis.

---

## Methodology

### The Climate-Labor Vulnerability Index (CLVI)

A composite index built to compare climate-social vulnerability across 192 countries on a 0 to 1 scale.

```
CLVI = Physical Exposure (50%) + Socioeconomic Vulnerability (50%)
```

**Physical Exposure** (from WRI): climate hazard exposure · lack of coping capacity · lack of adaptive capacity · susceptibility to harm

**Socioeconomic Vulnerability** (from World Bank + WIID): poverty ratio · Gini coefficient · Palma ratio · inverted GDP per capita

All variables normalised using Min-Max scaling before aggregation. Sensitivity analysis confirmed country rankings are stable across all alternative weightings tested (r > 0.99).

### The Protection Gap Score

```
Protection Gap = CLVI Score − Normalised SP Coverage Rate
```

A positive gap means a country is more vulnerable than it is protected. A negative gap means protection exceeds vulnerability, typical of high-income European countries.

---

## Key Findings

### Five Hypotheses Tested

| # | Hypothesis | Result | Key Statistic |
|---|---|---|---|
| H1 | Most vulnerable countries have least SP coverage | ✅ Supported | Spearman r = −0.743, p < 0.0001 |
| H2 | SP better designed for acute than slow-onset shocks | ❌ Reversed | Slow-onset 49.9% vs acute 30.7% |
| H3 | Women & vulnerable groups systematically excluded | ⚠️ Partial | Global gap 2pp; Southern Asia gap 11pp |
| H4 | Low-income countries lack fiscal capacity for SP | ✅ Supported | Low income 11.8% vs high income 90.6% |
| H5 | More vulnerable countries have less complete data | ⚠️ Reframed | 79 countries (41%) have no SP data at all |

### Headline Findings

- The geographic mismatch is systematic: Sub-Saharan Africa and South Asia score highest on vulnerability and lowest on coverage. Western Europe is the inverse.
- **21 countries** sit in the Red Zone: high climate exposure, near-zero SP coverage.
- Unemployment coverage is effectively **zero** across all income groups except high income, the benefit type most relevant to climate-disrupted livelihoods is the one that barely exists.
- The **Central African Republic**, the single most vulnerable country in the dataset (CLVI 0.748), has no SP data at all. The country the system most needs to monitor is invisible to it.
- When controlling for GDP in regression, climate vulnerability alone loses statistical significance. **Poverty and climate vulnerability are the same structural problem.**

---

## Visualisations

### Exploratory Data Analysis: Notebooks 04 & 05

Six charts produced using Matplotlib and Plotly:

1. **CLVI World Choropleth**: composite vulnerability scores across 192 countries
2. **Protection Gap Map** : where vulnerability exceeds provision (and vice versa)
3. **Quadrant Scatter** : physical exposure vs SP coverage, with four labelled zones (interactive HTML version available)
4. **SP Coverage by Benefit Type and Income Group** : grouped bar chart showing which benefits exist where
5. **Coverage Dot Plot by UN Region** : regional comparison across benefit types
6. **Radar Profiles :Red Zone Countries** : vulnerability fingerprints for the 10 countries with the largest protection gap

All charts are available in `outputs/charts/`. The interactive quadrant scatter is available as `chart3_quadrant_scatter.html`.

### Geographic Visualisations: Notebook 06

Six additional maps produced using **GeoPandas + Matplotlib**, offering publication-quality static output with precise layer control:

| Map | File | Description |
|---|---|---|
| 1 | `map1_clvi_geopandas.png` | CLVI world choropleth with case study annotations |
| 2 | `map2_protection_gap_geopandas.png` | Protection gap — diverging red/green scale |
| 3 | `map3_sp_coverage_geopandas.png` | SP coverage rate by country |
| 4 | `map4_invisibility_geopandas.png` | H5 — invisible countries coloured by CLVI score |
| 5 | `map5_redzone_geopandas.png` | Red Zone countries isolated and coloured by protection gap |
| 6 | `map6_sidebyside_geopandas.png` | CLVI and protection gap as a two-panel figure |

All GeoPandas maps are saved to `outputs/geopandas/`. The Natural Earth 110m shapefile is downloaded automatically on first run and cached in `data/raw/`.

---

## Interactive Dashboard

The full analysis is available as a **Tableau Public dashboard** with four interactive views:

- World map (toggle: CLVI score / SP coverage rate)
- Quadrant scatter (hover any country for full profile)
- SP coverage breakdown by benefit type and income group
- Financing gap: health expenditure as proxy for domestic SP capacity

All views filterable by UN region and income group. No account needed.

🔗 **[View on Tableau Public](https://public.tableau.com)** *(search: Climate Social Protection Gap — Sarah El Jamal)*

---

## Case Studies

Four countries selected by the data — not pre-selected:

| Country | CLVI | SP Coverage | Key Finding |
|---|---|---|---|
| Mozambique 🇲🇿 | 0.695 (99th pct) | 6% | Highest measured protection gap; health spending exists but SP delivery infrastructure does not |
| Somalia 🇸🇴 | 0.596 (97th pct) | 0% | Zero formal SP recorded; state absence and data absence are the same problem |
| Haiti 🇭🇹 | 0.608 (98th pct) | 6.6% | Gini 60.79 (96th pct); climate exposure compounding a redistribution crisis |
| Central African Republic 🇨🇫 | 0.748 (100th pct) | No data | Highest CLVI globally; completely invisible in SP monitoring architecture |

---

## Assumptions & Limitations

- **CLVI weighting:** The 50/50 split between physical and socioeconomic components is an assumption. Sensitivity analysis showed r > 0.99 across all alternative weightings: rankings are robust.
- **CAR protection gap:** The CAR has no SP coverage data. Its gap score uses median imputation. The CLVI is measured; the protection gap is inferred.
- **Health expenditure proxy:** H4 uses health expenditure (% GDP) as a proxy for domestic SP financing capacity, as OCHA humanitarian financing data was unavailable.
- **41% data gap:** The true Red Zone is likely larger than 21 countries. Countries with no SP data, including CAR, DRC, and Congo, could not be included in gap calculations.
- **Coverage ≠ adequacy:** ILO SDG 1.3.1 measures whether people receive *any* benefit,  not whether benefits are sufficient. The adequacy dimension of the gap is not quantified here.

---

## How to Run

```bash
# Clone the repo
git clone https://github.com/[your-username]/climate-social-protection.git
cd climate-social-protection

# Install dependencies
pip install pandas numpy matplotlib plotly scipy scikit-learn geopandas

# Run notebooks in order
jupyter notebook notebooks/01_data_loading.ipynb
```

Data files should be placed in `data/raw/` before running. Source links are included at the top of notebook 01.

Notebook 06 (`06_geopandas_maps.ipynb`) downloads the Natural Earth shapefile automatically on first run , no manual download required. All GeoPandas maps are saved to `outputs/geopandas/`.

---

## Policy Conclusions

The data points to three structural conclusions:

1. **The gap is not incidental , it is structural.** A correlation of −0.743 between vulnerability and coverage is the accumulated result of where social infrastructure investment has and hasn't happened over decades.
2. **Poverty and climate vulnerability are the same problem.** Designing climate resilience solutions without addressing the poverty trap will fail , in regression, the two are inseparable.
3. **What we cannot see, we cannot fix.** 79 invisible countries is a political problem, not a technical one. Until the monitoring architecture reaches the places that need it most, evidence-based intervention is structurally impossible there.

---

## About

**Sarah El Jamal** is a project manager and M&E professional with 11+ years of experience in international development (ILO, Oxfam, Morningstar Sustainalytics), now transitioning into data analysis. She has co-published peer-reviewed research on poverty and social protection and managed portfolios of over USD 180 million in development projects.

This project was completed as part of the Ironhack Data Analysis Bootcamp (March 2026).

🔗 [LinkedIn](https://www.linkedin.com/in/sarahel-jamal) · 📧 aljamalsara1@gmail.com
