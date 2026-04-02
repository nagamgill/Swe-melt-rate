# SNOTEL SWE Melt Rate Analysis (AWDB API)

This project analyzes Snow Water Equivalent (SWE) melt behavior across Colorado SNOTEL sites using the NRCS AWDB REST API.

The script pulls daily SWE data, identifies peak SWE timing, and quantifies melt progression for the current water year and selected historical comparison years.

---

## Overview

This workflow replaces spreadsheet-based analysis with a reproducible, scalable Python approach.

For each SNOTEL site, the script calculates:

- Peak SWE value
- Peak SWE date
- Current SWE
- Net SWE loss
- Percent SWE loss
- Days since peak
- Melt rate (SWE loss per day)

It also enables comparison against key historical years:
- 1981  
- 2002  
- 2012  
- 2015  
- 2017  
- 2018  

---

## Data Source

Data is retrieved from:

NRCS AWDB REST API  
https://wcc.sc.egov.usda.gov/awdbRestApi

Element used:
- `WTEQ` (Snow Water Equivalent, inches)
- Duration: `DAILY`

---

## Key Features

- Handles all Colorado SNOTEL sites (`*:CO:SNTL`)
- Automatically parses API JSON responses
- Aligns time series across different station periods of record
- Computes melt metrics consistently across sites
- Supports multi-year comparison for context

---

## Requirements

Install dependencies:

```bash
pip install pandas requests
