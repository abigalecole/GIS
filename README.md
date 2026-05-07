# ArcGIS Python Toolbox Scripts

A collection of Python scripts built with **ArcPy** for automating GIS workflows in ArcGIS Pro. These tools are designed to run as custom geoprocessing toolbox tools and streamline common fiber/telecom network mapping tasks.

---

## Scripts

### 1. FindNextNames — Next Available Feature ID Generator

Scans existing features within a selected zone and determines the next available ID for each structure type and span.

**What it does:**
- Takes a structures feature class, spans feature class, zone boundary, and an output table as inputs
- Spatially filters features that fall within the selected zone
- Parses structured IDs (e.g. `BP.ZONENAME.0000001`) using regex to find the current maximum number per type
- Calculates the next available ID for four structure types: Bore Pit (`BP`), Handhole (`HH`), Flowerpot (`DP`), and Cabinet (`V`)
- Also finds the next available Span ID (5-digit zero-padded)
- Writes results to an output table and prints a summary to the geoprocessing messages panel

**Use case:** Ensures consistent, conflict-free feature naming when adding new infrastructure to an existing network.

---

### 2. Label Creation — Splice Label Feature Class Creator

Generates a point feature class of splice label locations for handholes, enriched with equipment counts, fiber data, and address information.

**What it does:**
- Processes handholes that fall **inside** terminal boundaries first, then handles **outside** ones separately
- For each handhole location, it spatially joins and aggregates data from:
  - Splice features (size summaries, terminating fiber, HO-1 counts)
  - Equipment features (counts by subtype, e.g. splitters, patch panels, MSTs)
  - Address features (customer counts and formatted address lists)
- Outputs a point feature class placed at each handhole's location with all aggregated attributes attached
- Includes deduplication logic to avoid double-processing handholes that appear in multiple contexts

**Use case:** Automates the creation of map labels for splice enclosures, giving field crews and designers a quick summary of what's inside each handhole.

---

### 3. Set Definition Query — Multi-Map Definition Query Setter

Applies definition queries across multiple maps in an ArcGIS Pro project simultaneously, filtering permit boundary layers, FDA zones, and build zones to a selected area of interest.

**What it does:**
- Targets three maps at once: `Detailed Map`, `Grid Map`, and `Location Overview Map`
- Accepts a selected permit boundary layer (City, County, DOT, DNR, etc.) and a value to filter by
- Sets all *other* permit boundary layers to `ObjectID = -1` (effectively hiding them)
- Applies separate definition queries to an FDA layer and a Zone layer based on user-provided values
- Handles nested group layers via recursive search so it works regardless of layer organization

**Use case:** Quickly scopes an entire ArcGIS Pro project to a specific permit area and zone without manually updating definition queries in each map.

---

## Requirements

- ArcGIS Pro with a valid license
- Python 3.x (bundled with ArcGIS Pro)
- `arcpy` module (included with ArcGIS Pro installation)

## Notes

These scripts are intended to be registered as tools within an ArcGIS Pro custom toolbox (`.tbx` or `.atbx`). Each script reads its parameters via `arcpy.GetParameterAsText()` / `arcpy.GetParameter()` and is not designed for standalone command-line execution.
