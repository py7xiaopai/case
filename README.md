# JD vs Taobao — iPhone 16 Price Comparison Case Study

A professional data analytics case study demonstrating end-to-end capabilities in web data collection, anti-scraping, data cleaning, statistical analysis, and professional reporting.

## Overview

Collected and analyzed 310 iPhone 16 listings from China's two largest e-commerce platforms — JD.com and Taobao. The project showcases a breakthrough CDP-based anti-anti-scraping approach that bypasses 4 layers of protection (headless detection, SPA rendering, session token binding, API signing) using real browser Chrome DevTools Protocol connection.

## Key Findings

- **37% price gap** between JD.com (avg ¥5,312) and Taobao (avg ¥3,874) — primarily a product composition effect
- **9× more listings** on JD (278 vs 31), reflecting JD's stronger authorized seller ecosystem
- Taobao's lower prices reflect grey-market, carrier-locked, and second-hand devices
- Adjusting for product quality, the true cross-platform gap narrows to **10-15%**

## Deliverables

| File | Description |
|------|-------------|
| `jd_taobao_cleaned_data.csv` | Cleaned dataset (309 rows × 6 columns) |
| `JD_vs_Taobao_iPhone16_Price_Case_Study.pdf` | English case study report (14 pages) |
| `JD_vs_Taobao_iPhone16_Price_Case_Study_CN.pdf` | Chinese case study report (12 pages) |
| `jd_vs_taobao_case_study.html` | Interactive HTML dashboard with 4 charts |
| `jd_iphone16_price_chart.html` | JD.com single-platform price analysis |
| `jd_vs_tb_comparison.html` | Platform comparison visualization |

## Tech Stack

**Playwright (Python)** — Browser automation via CDP | **Python 3.12** — Core processing | **pandas / NumPy** — Data analysis | **ReportLab** — PDF generation | **matplotlib / Chart.js** — Visualizations

## Disclaimer

Data collected for educational and research purposes only. Not intended for commercial use.
