File Overview

All scripts are written in R and saved in plain text UTF-8 format.
The core script is:main_VAR.R – downloads data from FRED, constructs quarterly dataset, estimates the VAR(4), generates impulse response functions (IRFs), and computes forecast error variance decomposition (FEVD) shares.

Two time-span versions are provided:Version A: 1965Q3–1995Q3. Version B: 1974Q3–2024Q3

A version produces one dataset (cee.csv), one IRF figure (Fig1.pdf / Fig1.png), and one FEVD table (Table1.csv).
B version produces one dataset (ceenew.csv), one IRF figure (Fig1_new.pdf / Fig1_new.png), and one FEVD table (Table1_new.csv).

Required Packages:
library(fredr)
library(dplyr)
library(tidyr)
library(lubridate)
library(tsibble)
library(zoo)
library(readr)
library(purrr)
library(ggplot2)
library(vars)

API Key:Set your FRED API key before running: Sys.setenv(FRED_API_KEY = "YOUR_FRED_API_KEY")

How to Run
Step 1. Run the data construction block

This will:
Download quarterly data for
GDPC1, PCECC96, GDPDEF, GPDIC1, COMPRNFB, OPHNFB, CP, M2SL, FEDFUNDS

Convert to quarterly frequency (average or last value)

Log-transform real variables, deflate nominal ones

Compute growth rates and inflation

Save final dataset as cee.csv in the working directory

Each version is defined by its sample window:
Version	Period	Output File
A	1965Q3–1995Q3	cee.csv
B	1974Q3–2024Q3	ceenew.csv

Step 2. Run the VAR estimation block

This part:
Loads cee.csv

Selects 9 variables:y,c,p,i,w,prod,R,rp,m2_growth

Fits VAR(4) with constant term

Computes orthogonalized IRFs (bootstrap, 1000 runs)

Scales variables to percentage or pp (annualized)

Converts price response to inflation response

Saves IRF plot to:

Fig1.pdf (paper format) Fig1.png (for quick viewing)

Step 3. Run the FEVD computation block

This part:

Uses the companion form of VAR

Computes cumulative variance shares due to R shock at horizons 4, 8, 20 quarters

Reports results in Table1.csv

Output Files:
cee.csv:	Clean quarterly dataset used in estimation
Fig1.pdf / Fig1.png:	Impulse Response Functions (R shock)
Table1.csv:	Forecast Error Variance Decomposition table

Approximate Runtime:~2–3 minutes on a standard laptop. IRF bootstrap (1000 runs) is the main time-consuming part

Notes

All data are from FRED; no external proprietary data used.

Reproducibility may vary slightly with R version and random seed.

To ensure consistent results, seed is fixed via set.seed(123456).

Your Name – Your Email
(This replication is for academic coursework; not affiliated with original authors.)

AI Usage Statement

A generative AI assistant (ChatGPT) was used only for code explanation and documentation formatting.
All code, results, and interpretations were written and verified independently by the author.
