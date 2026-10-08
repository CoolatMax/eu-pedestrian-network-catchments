# Pedestrian Catchments & Network Service Area Analysis (QGIS)

![QGIS](https://img.shields.io/badge/QGIS-3.34_LTR-588240?logo=qgis&logoColor=white)
![Analysis](https://img.shields.io/badge/Methodology-Network_Service_Area_vs_Buffer-orange)
![CRS](https://img.shields.io/badge/Projection-EPSG%3A3035%20(ETRS89)-003399)

## Project Overview
This repository presents a spatial transit accessibility analysis in Brussels, evaluating pedestrian walking catchments around public transit stops at standard planning thresholds: **250 m (3-min walk)**, **500 m (6-min walk)**, and **800 m (10-min walk)**.

The project highlights the spatial discrepancy between **Network-Constrained Service Areas** (following walkable road/path topologies) and **Euclidean Straight-Line Buffers**, quantifying how naive spatial buffers overestimate real-world walking coverage in European urban environments.

## Objectives
1. **Network Cleaning for Pedestrians:** Filter OpenStreetMap infrastructure to retain only walkable segments (excluding high-speed motorways/trunks and including pedestrian footways/paths).
2. **Euclidean Buffer Generation:** Create multi-ring circular buffers around target transit stops at 250m, 500m, and 800m distances.
3. **Network Service Area Generation:** Execute QGIS Processing `Service Area (from Layer)` graph algorithms to map precise walkable coverage along street networks.
4. **Hull Polygon Extraction:** Enclose network reachability lines using Alpha Shapes / Concave Hulls to generate comparative service area polygons.
5. **Spatial Metrics & Cartography:** Quantify the spatial overestimation ratio ($\text{Area}_{\text{Buffer}} / \text{Area}_{\text{Network}}$) and produce a publication-ready A3 comparison map.

---

## Pedestrian Distance Thresholds & Walk Times

Based on standard European urban transit planning benchmarks (assuming an average walking speed of $4.8\text{ km/h} \approx 1.33\text{ m/s}$):

| Distance Threshold | Average Walk Time | Transit Category Target | Urban Mobility Role |
| :---: | :---: | :--- | :--- |
| **250 m** | ~3.0 min | Local Bus / Tram Stops | Immediate Access Zone |
| **500 m** | ~6.0 min | Metro Stations / BRT Corridors | Standard Transit Catchment (15-Min City Benchmark) |
| **800 m** | ~10.0 min | Commuter Rail / Central Hubs | Extended Pedestrian Shed |

---

## Workflow Implementation

### Step 1: Walkable Network Preparation
* **QGIS Manual Reference:** *Section 6.3.6 - Service Area (from layer)*
* Filtered road layers to retain pedestrian-accessible features:
  ```sql
  "highway" NOT IN ('motorway', 'motorway_link', 'trunk', 'trunk_link') 
  OR "foot" = 'yes'
