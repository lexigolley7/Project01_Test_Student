# Flock Safety ALPR Cameras and Reported Crime in Chicago: Individual EDA (Lexi)

**Course project:** CS418
**Author:** Alexandra Golley
**Scope:** Individual EDA component of a group research project

This folder contains my individual exploratory data analysis of the geographic relationship between mapped Flock Safety automated license plate reader (ALPR) cameras and police-reported crime in Chicago, using Chicago crime records from 2017 and 2026 (year-to-date).

> **Headline result:** Flock camera locations show some geographic overlap with areas of higher reported crime, but the overlap is not consistent. Grid cells with more mapped cameras do **not** reliably have more reported crime. These results are descriptive and do not show that cameras cause, prevent, or respond to crime.

---

## Table of contents

1. [Research questions](#research-questions)
2. [Data sources](#data-sources)
3. [Repository structure](#repository-structure)
4. [Getting started](#getting-started)
5. [Methods in brief](#methods-in-brief)
6. [Key findings](#key-findings)
7. [Limitations](#limitations)
8. [Ethics and responsible use](#ethics-and-responsible-use)
9. [Documentation](#documentation)
10. [References](#references)

---

## Research questions

The project started with four questions (shared with the group):

1. What is the relationship between the geographic distribution of Flock cameras and reported crime in Chicago?
2. Are areas with more Flock cameras associated with higher or lower levels of reported crime?
3. How do crime patterns in areas with Flock cameras compare with areas without them?
4. How has the relationship between Flock camera locations and reported crime changed between 2017 and 2026?

Question 4 could not be answered because the camera data is a current snapshot with no installation dates or historical locations. My analysis was revised to focus on **2026 year-to-date**:

> What relationship exists between the geographic distribution of Flock Safety ALPR cameras and reported crime in Chicago in 2026?

with three supporting questions: whether cameras sit in higher-crime areas, whether the relationship varies by crime type, and how reported crime varies across Chicago.

---

## Data sources

| File | Description | Size (local snapshot) | Source |
|---|---|---|---|
| `alpr_cameras.csv` | Mapped ALPR camera locations with a binary `flock` label | 101,085 rows × 4 cols | [NSM-Barii/flock-down](https://github.com/NSM-Barii/flock-down), derived from [OpenStreetMap](https://www.openstreetmap.org) |
| `chicago_crime_2017.csv` | Reported crimes, calendar year 2017 | 269,349 rows × 22 cols | [City of Chicago Data Portal](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-Present/ijzp-q8t2) (CPD CLEAR system) |
| `chicago_crime_2026.csv` | Reported crimes, Jan 1 – Sep 20, 2026 | 167,474 rows × 22 cols | City of Chicago Data Portal |

Things to know up front:

- The camera file covers many places worldwide, not just Chicago. In this snapshot, 75,614 records are labeled Flock and 25,471 are not.
- The 2026 crime file is a **partial year** (through September 20, 2026).
- All three files are static snapshots inspected in October 2026. Live sources update independently and will not match these row counts. The exact download queries and access dates were not recorded.

Full details (provenance, licensing, codebook, missingness) are in [`DATASHEET_LG.md`](DATASHEET_LG.md).

---

## Repository structure

```
.
├── README.md
├── data/
│   └── raw/
│       ├── alpr_cameras.csv
│       ├── chicago_crime_2017.csv
│       └── chicago_crime_2026.csv
├── notebooks/
│   ├── data_acquisition.ipynb
│   └── EDA_LG.ipynb              # Alexandra's EDA
├── figures/
│   ├── figure1_camera_crime_map.png
│   ├── figure2_camera_crime_scatter.png
│   └── figure3_camera_presence_comparison.png
└── docs/
    ├── DATASHEET_LG.md           # Data documentation / datasheet
    └── Narrative_Analysis_LG.md  # Written findings
```

> Only my files are shown; the group repository will contain additional work from teammates. Notebook paths use `../data/raw/` and `../figures/`, so notebooks are expected to live one level below the project root.

---

## Getting started

### Requirements

- Python 3.9+
- [Polars](https://pola.rs/) for data manipulation
- Matplotlib for plotting
- Jupyter

```bash
pip install polars matplotlib jupyter
```

### Running the analysis

1. Place the three CSV files in `data/raw/` (they are not downloaded by the notebooks).
2. Create a `figures/` folder in the project root if it does not exist.
3. Launch Jupyter and run `notebooks/EDA_LG.ipynb` from top to bottom:

```bash
jupyter notebook notebooks/EDA_LG.ipynb
```

---

## Methods in brief

**Camera subset.** Cameras were filtered to a Chicago bounding box (`41.64 ≤ lat ≤ 42.03`, `-87.94 ≤ lon ≤ -87.52`), giving 1,101 records, of which 482 are labeled Flock. A rectangle approximates, but is not identical to, the city boundary.

**Crime subset.** Rows missing latitude or longitude were excluded from maps and grid analysis only (304 of 167,474 in 2026, leaving 167,170). They remain in the full files.

**Grid comparison.** Both Flock cameras and 2026 crimes were assigned to cells on a `0.02°` latitude/longitude grid using `floor(coordinate / 0.02)`. Counts per cell were left-joined from the crime grid to the camera grid, with missing camera counts filled as zero. The result, `camera_crime_grid`, has 206 cells. Because the join starts from the crime grid, camera-only cells with no crime records are not included.

**Supporting analysis.** Crime-type counts, monthly counts, and arrest rates by crime type for 2017 and 2026.

---

## Key findings

1. **Some overlap, not consistent.** On a map, some Flock cameras sit in areas of denser reported crime, but many are outside the densest clusters, and crime appears in areas with no mapped cameras.
2. **No clear trend across grid cells.** In a scatter plot of camera count vs. crime count per cell, cells with few or no cameras show a wide range of crime counts, including some of the highest. No clear upward or downward pattern appears as camera count rises.
3. **Crime occurs with and without matched cameras.** A zero in this analysis means no camera was *mapped* in that cell, not that none exists.
4. **2017 → 2026 change cannot be determined.** There is no historical camera data, and 2026 is a partial year, so raw totals are not comparable.

Supporting context (2026 YTD):

| Crime type | Records | Arrest rate |
|---|---:|---:|
| Theft | 36,122 | 8.99% |
| Battery | 30,875 | 19.29% |
| Criminal damage | 18,368 | 4.59% |
| Assault | 15,194 | 13.66% |
| Motor vehicle theft | 13,211 | 3.72% |

Monthly reported crime rises from about 17,000 in January to a peak of about 21,200 in July. September (12,463) is incomplete. Narcotics has by far the highest arrest rate (94.93%). An arrest flag is not a conviction, and these figures say nothing about whether Flock cameras affect arrests.

Full write-up with figures and alternative explanations: [`Narrative_Analysis_LG.md`](Narrative_Analysis_LG.md).

---

## Limitations

- **Camera coverage:** OpenStreetMap is volunteer-maintained, so cameras may be missing, stale, or mislabeled. `flock = 0` means "not labeled Flock," not "not Flock."
- **No installation dates or operational status** in the camera data.
- **Bounding-box filter** is not the official city boundary.
- **Police-reported crime only;** it reflects reporting and enforcement patterns, not all incidents.
- **Missing coordinates:** 4,413 rows in 2017 and 304 in 2026.
- **Partial 2026 coverage** (through Sep 20).
- **Grid choice:** `0.02°` is an exploratory unit, not a fixed distance or an administrative boundary.
- **No statistical testing:** conclusions are visual. No correlation coefficient or model was computed.
- **No causal inference** in either direction.

---

## Ethics and responsible use

- ALPR cameras are surveillance infrastructure. Mapped points should not be used to make claims about individuals' movements or the surveillance experienced by any specific community.
- Crime records involve real people (victims, witnesses, arrestees). A record is not proof of guilt. Block-level locations are approximate and must not be used to derive specific addresses.
- Avoid using these results to label neighborhoods as inherently criminal or to justify decisions about individuals or places.

---

## Documentation

| Document | What it covers |
|---|---|
| [`DATASHEET_LG.md`](DATASHEET_LG.md) | Motivation, composition, collection, preprocessing, uses, licensing, maintenance, codebook, raw-to-analysis recipe, FAIR notes |
| [`Narrative_Analysis_LG.md`](Narrative_Analysis_LG.md) | Main findings, evidence, figures, alternative explanations, limitations |
| [`notebooks/EDA_LG.ipynb`](notebooks/EDA_LG.ipynb) | Code for all analysis and figures |

---

## Licensing and attribution

- Camera locations derive from OpenStreetMap, which is distributed under the [ODbL](https://opendatacommons.org/licenses/odbl/). Attribution and share-alike terms may apply. See the [OSM copyright page](https://www.openstreetmap.org/copyright).
- Chicago crime data is published by the City of Chicago under its [Terms of Use](https://data.cityofchicago.org/terms). Review them before redistributing the raw files.

## References

1. NSM-Barii, *flock-down*: https://github.com/NSM-Barii/flock-down
2. OpenStreetMap copyright and license: https://www.openstreetmap.org/copyright
3. Open Data Commons ODbL: https://opendatacommons.org/licenses/odbl/
4. City of Chicago Data Portal, *Crimes - 2001 to Present*: https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-Present/ijzp-q8t2
5. City of Chicago Data Portal Terms of Use: https://data.cityofchicago.org/terms
6. Chicago Police Department data and statistics: https://www.chicagopolice.org/data-statistics/
