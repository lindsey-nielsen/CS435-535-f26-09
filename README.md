# African Lion Habitat and Movement Analysis

CS 435/535 — Large-Scale Data Analysis
Lindsey Nielsen & Juliet Bremang
Fall 2026

## Overview

This project uses PySpark to explore how African lions select habitat in relation to human settlements, vegetation, water, terrain, season, and sex.

## Dataset

- **Source:** [Coexistence from a lion’s perspective](https://zenodo.org/records/10719203)
- **File:** `Lion_collar_data_processed.csv`
- **Size:** Approximately 1.1 GB
- **Study area:** Ngorongoro Conservation Area, Tanzania
- **Animals:** 22 GPS-collared African lions
- **Format:** CSV with 17 documented fields and an additional exported row index

Each row describes the endpoint of an observed movement or a simulated alternative. For each observed movement, researchers generated 50 alternative endpoints based on the lion’s movement patterns. These represent places the lion could plausibly have reached from the same starting point.

The `case_` column marks observed endpoints (`TRUE`) and simulated alternatives (`FALSE`). The `step_id_` column groups each observed endpoint with its alternatives. Comparing these locations helps account for the habitat available near the lion during each movement.

## Research Questions

1. Do lions choose locations closer to human settlements at night, and does this change between wet and dry seasons?
2. When lions move near settlements, do they choose locations with more forest or shrub cover?
3. Do lions choose locations closer to water sources during the dry season?
4. Do male and female lions differ in their use of rugged terrain?
5. Do lions favor different land-cover types during long-distance dispersal?
6. How does the density of nearby settlements relate to habitat selection when distance to the nearest settlement is similar?

## Main Variables

| Columns | Meaning |
| --- | --- |
| `id`, `step_id_`, `case_` | Lion identifier, movement comparison group, and observed versus simulated endpoint |
| `Dry_season`, `Night` | Wet/dry season and day/night indicators |
| `Male`, `Dispersal_movement` | Sex and whether a movement belongs to long-distance dispersal |
| `Land_cover`, `ForShrub` | Land-cover category and percentage of forest/shrub cover within 50 meters |
| `Dist_human` | Distance to the nearest boma or building, in meters |
| `Dens_human_1k`, `Dens_human_3k`, `Dens_human_5k` | Measures of nearby boma/building density within 1, 3, and 5 kilometers |
| `Dist_river`, `Dist_water` | Distances to the nearest river and water source, in meters |
| `TRI` | Terrain ruggedness index |
| `EVI` | Satellite-derived vegetation greenness, summarized by season |

A **boma** is a livestock enclosure. Land-cover codes are `1` for open land, `2` for forest, and `3` for shrubland. Binary indicators use `1` for dry season, nighttime, male, and dispersal, respectively.

## Requirements

- Access to the CS cluster and HDFS
- Python with PySpark, Jupyter/IPython kernel support, and Matplotlib
- VS Code with Remote SSH, Python, and Jupyter extensions when using VS Code

## Dataset Setup

The dataset is already available in HDFS at:

```text
/user/lindseynielsen/data/Lion_collar_data_processed.csv
```

This path is used in the notebook:

```python
data_path = "hdfs:///user/lindseynielsen/data/Lion_collar_data_processed.csv"
```

Downloading the CSV into the project’s local `data` folder is optional. The notebook reads directly from the existing HDFS copy.

If you download a local copy, add this entry to the root `.gitignore` file:

```gitignore
/data/Lion_collar_data_processed.csv
```

## Running the Notebook

1. Connect to the CS cluster and open the project folder.
2. Open `african_lion_analysis.ipynb`.
3. Select the cluster’s Python kernel with PySpark available.
4. Run the notebook cells in order, starting with imports and Spark session creation.
5. Stop the Spark session when finished with `spark.stop()`.

The initial setup and loading code is:

```python
# Import Spark tools and plotting libraries
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
import matplotlib.pyplot as plt

# Create or reuse the Spark session
spark = (
    SparkSession.builder
    .appName("African Lion Analysis")
    .getOrCreate()
)

# Load the dataset from HDFS
data_path = "hdfs:///user/lindseynielsen/data/Lion_collar_data_processed.csv"

lion_df = (
    spark.read
    .option("header", True)
    .option("inferSchema", True)
    .csv(data_path)
)

# Inspect the schema and sample rows
lion_df.printSchema()
lion_df.show(5)
```

## Analysis Approach

Data preparation will check inferred types, missing values, category codes, and movement groups. Observed endpoints will be compared with alternatives from the same `step_id_`. For example, the observed distance to a building can be compared with the median distance among that step’s alternatives.

Spark will perform filtering, grouping, joins, and aggregation. Small aggregated results will be collected for plotting. Where appropriate, results will also be summarized per lion so individuals with more recorded movements do not dominate comparisons.

The processed CSV provides day/night and seasonal indicators. Exact timestamps and geographic coordinates are absent from its schema. The study follows 22 lions, and repeated movements from the same lion are related observations. Conclusions should describe patterns within this study population.

## Project Status

Analysis is in progress. Findings and plots will be added after the notebook has been run and its outputs checked.

## Attribution

Jansson, I., Parsons, A., Singh, N., Faust, L., Kissui, B., Mjingo, E., Sandström, C., and Spong, G. *Coexistence from a lion’s perspective: Movements and habitat selection by African lions (Panthera leo) across a multi-use landscape.*

**Dataset DOI:** https://doi.org/10.5061/dryad.j6q573nnb

Variable definitions and details of simulated alternatives are documented in the [dataset README](https://zenodo.org/records/10719203/preview/README.md?include_deleted=0).
