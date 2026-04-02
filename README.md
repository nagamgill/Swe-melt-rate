# Colorado SNOTEL SWE Melt Analysis

Python-based analysis of Colorado SNOTEL Snow Water Equivalent (SWE) using the USDA NRCS AWDB REST API, with optional integration of current iMAP peak and melt-out CSV exports.

This workflow is designed to support quantitative melt-season analysis and public-facing summary writing, including WSOR and news release language.

---

## What this project does

The script pulls daily SWE (`WTEQ`) for active Colorado SNOTEL and SNTLT stations and computes:

- seasonal peak SWE
- peak date
- current SWE
- net loss from peak
- percent loss from peak
- melt rate since peak
- daily statewide SWE change
- strongest statewide melt days
- strongest station-level melt events

It also supports:

- historical comparison against selected years
- common-station comparison for apples-to-apples context
- 2026 full-network snapshot diagnostics
- optional integration of iMAP exports for:
  - peak timing departures
  - melt-out departures
  - percentile and record context

---

## Data sources

### 1. AWDB REST API
Used for actual daily SWE behavior.

Source:
`https://wcc.sc.egov.usda.gov/awdbRestApi`

Element:
- `WTEQ` = Snow Water Equivalent

Duration:
- `DAILY`

AWDB is used for:
- daily time series
- seasonal peak detection
- decline rates
- statewide daily melt diagnostics

### 2. iMAP CSV exports
Used for reference and contextual fields, not daily melt behavior.

Expected filenames in the same folder as the script:
- `Swe water year peak.csv`
- `Swe melt out.csv`

These files are useful for:
- peak date reference
- melt-out date reference
- departure from POR median timing
- percentile rank
- record status
- 1991–2020 and POR median timing fields

These files are **not** used in place of AWDB daily SWE.

---

## Why this workflow exists

Daily AWDB data shows **what actually happened**:
- when SWE peaked
- how quickly it declined
- which days drove the strongest statewide loss

The iMAP exports provide **reference context**:
- how unusual peak timing was
- how unusual melt-out timing was
- percentile / record framing

Together, they support quantitative statements like:
- how much SWE has been lost since peak
- how widespread daily SWE loss was during March
- how many sites are near or effectively melted out
- how current peak/melt timing compares to historical norms

---

## Key definitions

### Strict zero
A site with `current_swe == 0`.

### Effective melt-out
A site with `current_swe <= 0.5"`.

This is more practical than strict zero because trace accumulation, pooling, and delayed editing can leave a site with 0.1–0.5" even when it is functionally snow-free.

### Near melt-out
A site with `current_swe <= 5.0"`.

This is useful for identifying sites that are essentially down to remnant snowpack.

### Peak timing anomaly
Observed peak date minus the site’s climatological median peak date from AWDB central tendencies.

- negative = early
- positive = late

---

## Current workflow structure

The script has two main branches.

### 1. Historical comparison
The script pulls selected analog years and computes:
- statewide metrics for all available sites
- common-station metrics for sites present across all selected years

This is useful for historical context but should not replace the full-network 2026 view.

### 2. 2026 full-network snapshots
The script runs one or more snapshot end dates for 2026, for example:
- `2026-03-30`
- `2026-04-01`

This is the best output for:
- current statewide melt behavior
- strongest decline days
- near/effective melt-out counts
- write-up support

---

## Recommended usage

### Pre-storm baseline
Use a snapshot ending on `2026-03-30` for the cleanest March melt picture.

### Post-storm status
Use a later snapshot like `2026-04-01` to show how storm snow temporarily changed surface conditions.

### Historical context
Use the common-station metrics only as supporting context.

---

## Requirements

Install dependencies:

```bash
pip install pandas requests
