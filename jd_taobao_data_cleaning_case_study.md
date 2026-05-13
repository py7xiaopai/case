---
title: E-Commerce Price Data Cleaning & Analysis - JD.com vs Taobao iPhone 16
client: Upwork Portfolio / Self-Project
date: 2026-05-13
tags: data cleaning, data quality, price analysis, e-commerce, cross-platform comparison
platforms: JD.com, Taobao.com
data_version: JD-v1.0 / TB-v1.0
cleaning_pipeline: CL-v2.0
---

# E-Commerce Price Data Cleaning & Analysis
## JD.com vs Taobao - iPhone 16 Series

> A professional data cleaning case study demonstrating the full pipeline: raw data audit, standardized filtering, outlier detection, quality validation, and structured delivery.

---

## 1. Project Overview

**Objective**: Extract, clean, and compare iPhone 16 price data from two major Chinese e-commerce platforms - JD.com and Taobao. Deliver a structured, quality-validated dataset with cross-platform comparison.

**Data Sources**:
- **JD.com** (search.jd.com) - 279 raw records extracted via browser-based automation
- **Taobao.com** - 31 records extracted for cross-platform comparison
- **Collection Date**: 2026-05-12 (single batch)

---

## 2. Data Audit (Pre-Cleaning)

Before any cleaning, a systematic audit was performed on the raw data to understand its quality baseline.

### JD.com Raw Data Profile

| Metric | Value |
|--------|-------|
| Raw records | 279 |
| Unique price values | 186 / 279 |
| Duplicate rate | 33.3% |
| Price range (raw) | ¥2,649.0 ~ ¥18,252.0 |
| Mean price (raw) | ¥5,358.33 |
| Median price (raw) | ¥5,098.0 |

### Outlier Detection

**IQR Method**: Q1=¥3,759.0, Q3=¥6,499.0, IQR=¥2,740.0
Normal range: ¥-351 ~ ¥10,609
Statistical outliers: 4 items
  - ¥10,798
  - ¥10,874
  - ¥10,999
  - ¥18,252

**Business Rule Check** (¥2,500~¥15,000 - iPhone 16 reasonable price range):
Out-of-range items: 1 (¥18,252)
These represent non-standard listings (bundles, bulk packages) that would distort average price analysis.

### Taobao Raw Data Profile

| Metric | Value |
|--------|-------|
| Raw records | 31 |
| Price range | ¥2,540.0 ~ ¥6,599.0 |
| Mean price | ¥3,873.84 |
| Median price | ¥3,488.0 |

---

## 3. Data Cleaning Pipeline

The cleaning pipeline follows a structured, auditable process:

```
Raw Data -> Step 1: Price Filter -> Step 2: Outlier Flag -> Step 3: Quality Validate -> Clean Dataset
```

### Step 1: Business Rule Filtering

| Rule | Detail |
|------|--------|
| Price range | ¥2,500 ~ ¥15,000 (iPhone 16 reasonable market range) |
| Rationale | Excludes accessories (cases, chargers, film) and non-standard bundles |
| Applied to | Both JD and Taobao datasets |

### Step 2: Outlier Flagging

Statistical and business-rule hybrid approach:
- Items flagged if outside IQR x 1.5 range OR outside business price range
- Flagged items are recorded but only biz-rule violations are removed
- This preserves borderline statistical outliers for manual review

### Step 3: Quality Validation

Five-dimensional quality scoring before final delivery.

---

## 4. Cleaning Results

### JD.com: Before vs After

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| Record count | 279 | 278 | 1 removed (0.4%) |
| Min price | ¥2,649.0 | ¥2,649.0 | - |
| Max price | ¥18,252.0 | ¥10,999.0 | Outlier removed |
| Mean price | ¥5,358.33 | ¥5,311.95 | More representative |
| Median price | ¥5,098.0 | ¥5,083.0 | Stable |

**Removed items**: ¥18,252

### Taobao: Summary

- 31 valid records after cleaning
- Prices: ¥2,540.0 ~ ¥6,599.0
- Average: ¥3,873.84

---

## 5. Cross-Platform Comparison

| Metric | JD.com | Taobao | Difference |
|--------|--------|--------|------------|
| Valid products | 278 | 31 | +247 on JD |
| Min price | ¥2,649.0 | ¥2,540.0 | ¥+109.0 |
| Max price | ¥10,999.0 | ¥6,599.0 | ¥+4,400.0 |
| Mean price | ¥5,311.95 | ¥3,873.84 | ¥+1,438.1099999999997 |
| Median price | ¥5,083.0 | ¥3,488.0 | ¥+1,595.0 |

### Price Distribution by Platform

| Bucket | JD.com | Taobao |
|--------|--------|--------|
| ¥2,500~¥3,500 |  44 ############ |  16 #################### |
| ¥3,500~¥4,500 |  68 #################### |   8 ########## |
| ¥4,500~¥5,500 |  48 ############## |   3 ### |
| ¥5,500~¥6,500 |  51 ############### |   3 ### |
| ¥6,500~¥8,000 |  49 ############## |   1 # |
| ¥8,000~¥11,000 |  18 ##### |   0   |

### Key Findings

1. **JD.com average price is 37.1% higher than Taobao**
2. **JD.com has 9x more valid iPhone 16 listings** - suggesting more structured first-party seller ecosystem
3. **Taobao's lower average (¥3,873.84 vs ¥5,311.95) is consistent with a higher proportion of non-retail-channel listings** (second-hand, grey market, carrier-locked devices)
4. **JD.com self-operated (ziying) listings**: 10 items, prices ¥3,149 ~ ¥10,999, representing first-party retail pricing baseline

---

## 6. Data Quality Report

### Five-Dimensional Quality Scoring

| Dimension | Definition | JD Score | TB Score |
|-----------|-----------|----------|----------|
| Completeness | No missing values in key fields | 100.0 | 100.0 |
| Accuracy | Values within expected business range | 100.0 | 100.0 |
| Consistency | Uniform format and type across records | 100.0 | 100.0 |
| Uniqueness | Proportion of non-duplicate records | 66.5 | 93.5 |
| Timeliness | Single-batch, same-day collection | 100.0 | 100.0 |
| **Overall** | | **113.3** | **118.7** |

### Cleaning Effect Summary

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| Price outliers | 1 (0.4%) | 0 (0.0%) | Removed |
| Records available | 279 | 278 | Clean dataset |
| Price range | ¥2,649.0~¥18,252.0 | ¥2,649.0~¥10,999.0 | Normalized |

---

## 7. Field Dictionary

```
Field Name      | Type     | Description                  | Cleaning Rule
----------------|----------|------------------------------|------------------------------
platform        | string   | Data source platform         | Mapped: JD.com / Taobao
item_id         | string   | Unique item identifier       | Assigned: JD-XXXX / TB-XXXX
price           | float64  | Cleaned price in CNY         | Range filter ¥2,500~¥15,000
is_self_operated| boolean  | JD ziying flag               | Based on title analysis
crawl_date      | date     | Collection date              | 2026-05-12 (single batch)
cleaning_version| string   | Pipeline version ID          | CL-v2.0
```

---

## 8. Deliverables

| File | Description |
|------|-------------|
| `jd_taobao_cleaned_data.csv` | Cleaned dataset (309 rows x 6 columns) |
| `jd_vs_taobao_case_study.html` | Interactive HTML dashboard with visualizations |
| `JD_vs_Taobao_iPhone16_Price_Case_Study.pdf` | PDF report for client presentation |
| `jd_vs_taobao_upwork_case.md` | Upwork portfolio case description |

---

## 9. Technical Notes

### Data Collection Approach

Data was collected using **browser-based extraction** with authenticated session reuse:
- **Playwright** via Chrome DevTools Protocol (CDP) to connect to a real Chrome instance
- This approach handles **SPA-rendered pages** (React on JD.com) without headless detection
- Session authentication is preserved naturally via the browser's existing cookies
- All data collected in a single batch for temporal consistency

### Cleaning Pipeline Script

```python
def run_cleaning_pipeline(df_raw):
    '''CL-v2.0: Audit -> Filter -> Flag -> Validate -> Export'''
    audit = data_audit(df_raw)                        # Step 0: Diagnostic
    df = df_raw[(df_raw['price'] >= 2500) &           # Step 1: Biz filter
                (df_raw['price'] <= 15000)]
    df['outlier_flag'] = detect_outliers(df, 'price') # Step 2: Statistical flag
    quality = quality_score(df)                        # Step 3: 5-dim scoring
    export(df, 'jd_taobao_cleaned_data.csv')          # Step 4: Deliver
    return df, {'audit': audit, 'quality': quality}
```

---

*Cleaning Pipeline v2.0 - Generated on 2026-05-13*