# JD vs Taobao — iPhone 16 Price Comparison Case Study
# 京东 vs 淘宝 — iPhone 16 价格对比案例研究

> **EN** — A professional data analytics case study: web scraping, anti-anti-scraping, data cleaning, cross-platform price comparison, and professional reporting.
>
> **中文** — 专业数据分析案例：网页采集、反反爬、数据清洗、跨平台价格对比及专业报告输出。

---

## Overview / 项目概览

Collected and analyzed **310 iPhone 16 listings** from China's two largest e-commerce platforms. The project showcases a breakthrough **CDP-based anti-anti-scraping** approach that bypasses 4 layers of protection (headless detection, SPA rendering, session token binding, API signing).

采集并分析了中国两大电商平台 **310 条 iPhone 16 商品数据**。项目展示了独创的 **CDP 反反爬方案**，绕过了无头检测、SPA 渲染、会话令牌绑定、API 签名四层防护。

| Metric / 指标 | JD.com / 京东 | Taobao / 淘宝 |
|---|---|---|
| Valid Listings / 有效商品 | 278 | 31 |
| Avg Price / 均价 | ¥5,312 | ¥3,874 |
| Median Price / 中位价 | ¥5,083 | ¥3,488 |
| Price Range / 价格区间 | ¥2,649 ~ ¥10,999 | ¥2,540 ~ ¥6,599 |

## Key Findings / 核心发现

- **37% price gap** — primarily a product composition effect (retail vs grey-market)
- **9× more listings** on JD — reflecting stronger authorized seller ecosystem
- True cross-platform gap narrows to **10-15%** when comparing like-for-like quality
- JD 价格高 37%，主要源于商品结构差异（零售正品 vs 水货二手）
- 京东商品数 9 倍于淘宝，反映更强的授权卖家生态
- 同品质对比下，实际价差缩小至 10-15%

## Deliverables / 交付物

| File / 文件 | Description / 说明 |
|---|---|
| `jd_taobao_cleaned_data.csv` | Cleaned dataset (309×6) / 清洗后数据集 |
| `JD_vs_Taobao_iPhone16_Price_Case_Study.pdf` | English report (14p) / 英文报告 |
| `JD_vs_Taobao_iPhone16_Price_Case_Study_CN.pdf` | Chinese report (12p) / 中文报告 |
| `jd_vs_taobao_case_study.html` | Interactive dashboard / 交互式仪表盘 |

## Tech Stack / 技术栈

**Playwright (Python)** — CDP browser automation · **Python 3.12** — Core pipeline · **pandas / NumPy** — Data analysis · **ReportLab** — PDF generation · **matplotlib / Chart.js** — Visualization

## Disclaimer / 声明

Data collected for educational and research purposes only.
数据仅用于教育及研究目的。
