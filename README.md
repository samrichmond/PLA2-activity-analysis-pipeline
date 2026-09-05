# PLA2 Activity Analysis Pipeline

**Author:** Samantha Richmond
**Tools:** Python | Pandas | NumPy | Matplotlib | Google Colab

---

## Overview

An end-to-end data engineering pipeline for processing and visualizing fluorescence plate reader data from phospholipase A2 (PLA2) enzyme activity assays.

Developed to analyze kinetic fluorescence data from a Tecan Infinite F200 Pro plate reader (Magellan software export) to study PLA2 stimulation activity of Pat1, a patatin-like phospholipase from the intracellular pathogen Rickettsia parkeri that plays a role in host vacuole escape during infection.

---

## The Problem This Solves

Magellan software exports plate reader data as a non-standard Excel file — one plate layout matrix per time point, stacked sequentially with no consistent headers or well labels. Manual processing of this format across 25 time points and 11 samples was time-consuming and error-prone.

This pipeline automates the entire process from raw export to publication-ready figure.

---

## Pipeline Architecture

Cell 1: Data Loading and Inspection
→ Mount Google Drive
→ Load raw Excel file
→ Inspect structure

Cell 2: Parsing Time Blocks
→ Detect timestamp rows automatically
→ Extract fluorescence values per well
→ Map wells to sample identities
→ Apply time offset correction

Cell 3: Blank Correction and Protein Data
→ Calculate time-matched blank means
→ Subtract blank from every sample at every time point
→ Define protein amounts for normalization

Cell 4: Means, Normalization and Graphing
→ Calculate mean and SD across triplicates
→ Extract endpoint time point (90 min)
→ Normalize by nmol protein loaded
→ Generate publication-quality bar graph

Cell 5: Reusable Graphing Function
→ Parameterized function for any experiment
→ Supports multiple conditions (e.g. ±DDM)
→ Optional group labels, separators, legends
→ Auto-saves figures to Google Drive

---

## Key Features

✓ Automatic parsing of non-standard Excel exports
✓ Time-matched blank correction across kinetic reads
✓ Protein normalization by nmol loaded (accounts for molecular weight and volume)
✓ Configurable time offset for late-start experiments
✓ Reusable graphing function for multiple experiments
✓ Publication-quality figures saved automatically
✓ Modular design — each cell has one job
✓ Raw data never modified (immutability principle)

---

## Example Output

PLA2 Activity Bar Graph

Bar graph showing blank-corrected, protein-normalized PLA2 stimulation activity at 90 minutes across SEC purification fractions of Pat1 WT (J series) and catalytically dead S50A mutant (N series). Error bars represent standard deviation of technical triplicates.

---

## How to Use

Setup
In Cell 1, update these two variables:

FILE_PATH = '/content/drive/MyDrive/your_folder/your_data.xlsx'
OUTPUT_FOLDER = '/content/drive/MyDrive/your_folder/figures'

Configure Your Plate Layout
In Cell 2, update SAMPLE_MAP to match your plate layout:

SAMPLE_MAP = {
'E': [
('Your_Sample_1', [0, 1, 2]),
('Your_Sample_2', [3, 4, 5]),
],
}

Configure Protein Data
In Cell 3, update protein_data with your sample concentrations and molecular weights.

Run All Cells
Runtime → Run all (Ctrl+F9)

---

## Data Format

Input: Tecan Magellan Excel export

Row 1:  0s  ← Timestamp
Row 2:  NaN ← Well A (empty)
Row 6:  data ← Well E (samples)
Row 7:  data ← Well F (samples)
Row 8:  data ← Well G (partial)
Row 9:  NaN ← Well H (empty)
Row 10: 300s ← Next timestamp
...repeats for each time point

A sample dataset (sample_data.xlsx) is included with anonymized values matching the expected format.

---

## Dependencies

All pre-installed in Google Colab:

pandas
numpy
matplotlib
re (standard library)
os (standard library)

---

## Future Development

→ Google Sheets configuration system for variable plate layouts
→ Kinetic curve plotting
→ Standard curve integration for RFU to nmol product conversion
→ Batch processing multiple files

---

## Scientific Context

Pat1 (Phospholipase A2) is a patatin-like phospholipase from Rickettsia parkeri, an obligate intracellular bacterial pathogen. Pat1 is thought to aid bacterial escape from the host vacuole during infection by stimulating phospholipase A2 activity at the vacuolar membrane.

This pipeline was developed to systematically compare PLA2 stimulation activity across:

SEC purification fractions (different oligomeric states)
Protein truncation constructs (structure-function studies)
Experimental conditions (e.g. ±detergent DDM)
Multiple purification batches
Note: Sample data included in this repository uses anonymized values. Real experimental data is property of the Welch Lab, UC Berkeley.
