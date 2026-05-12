---
title: 京东 vs 淘宝 iPhone 16 价格数据采集与对比分析
client: Upwork Portfolio / Self-Project
date: 2026-05-12
tags: web scraping, data analysis, price intelligence, e-commerce, anti-anti-scraping, CDP, Playwright
platforms: JD.com, Taobao.com
tools: Playwright, Chrome DevTools Protocol, Python, Chart.js
difficulty: ★★★★★ (京东淘宝均为国内反爬最强电商之一)
---

# 京东 vs 淘宝 iPhone 16 价格数据采集与对比分析

> 一份展示高难度 Web 数据采集 + 数据清洗 + 可视化分析能力的 Upwork 案例

---

## 1. 项目背景

客户需要获取京东（JD.com）和淘宝（Taobao.com）两大中国电商平台上 iPhone 16 系列手机的真实价格数据，并进行跨平台对比分析，为竞品定价策略提供数据支撑。

**核心挑战**：京东和淘宝是中国反爬防护最严格的电商平台，分别采用了多层反爬机制：

| 防护层 | 京东 | 淘宝 |
|--------|------|------|
| SPA 客户端渲染 | ✅ React SPA（仅 HTML 空壳） | ✅ 动态加载 |
| headless 浏览器检测 | ✅ Canvas/WebGL/UA 检测 | ✅ 反自动化检测 |
| 会话令牌指纹绑定 | ✅ `_t` cookie 绑定指纹+IP | ✅ 登录态保护 |
| API 签名保护 | ✅ h5st + x-api-eid-token | ✅ 加密参数 |
| 频率限制 | ✅ 短时间多次请求触发拦截 | ✅ 滑块验证码 |
| CSS 类名混淆 | ✅ CSS Modules 哈希类名 | ✅ 同 |

---

## 2. 技术方案

### 2.1 核心思路：真实浏览器 CDP 直连

传统方案（requests/headless Chrome/Cookie 注入）因京东的 `_t` 令牌指纹绑定机制全部失效。最终采用 **Chrome DevTools Protocol（CDP）直连用户真实浏览器** 的方案：

```
用户真实 Chrome（完整指纹 + 登录态）
 → --remote-debugging-port=9222 启动调试模式
 → Playwright connect_over_cdp() 建立 CDP 连接
 → 复用 browser.contexts[0]（Cookie + 指纹天然继承）
 → 新标签页导航至搜索页
 → 等待 SPA 渲染完成
 → DOM 层面提取商品数据
```

### 2.2 技术栈

| 工具 | 用途 |
|------|------|
| **Playwright** (Python) | CDP 连接 + 浏览器自动化控制 |
| **Python 3.12** | 数据采集脚本、清洗、分析 |
| **Chart.js** | 价格可视化（4 张交互式图表） |
| **Linux (Ubuntu/GNOME)** | 运行环境，DISPLAY=:1 X11 |
| **Chrome 148** | 带完整指纹的真实浏览器 |

### 2.3 爬取流程

```python
# 1. 启动 Chrome（远程调试模式）
chrome --remote-debugging-port=9222 --user-data-dir=/tmp/profile

# 2. Playwright CDP 连接
browser = await playwright.chromium.connect_over_cdp("http://localhost:9222")
page = await browser.contexts[0].new_page()

# 3. 导航 + 等待 SPA 渲染
await page.goto(url, wait_until="domcontentloaded")
await page.wait_for_timeout(12000)  # SPA 渲染等待

# 4. DOM 提取（不依赖哈希类名）
products = await page.evaluate("""
    () => document.querySelectorAll('[data-sku]').map(card => ({
        sku: card.getAttribute('data-sku'),
        text: card.innerText.replace(/\\s+/g, ' ').trim()
    }))
""")
```

### 2.4 京东反爬攻破记录

| 反爬措施 | 尝试方案 | 结果 |
|----------|---------|------|
| SPA 空壳 | requests/httpx | ❌ 仅 HTML 空壳 |
| headless 检测 | DrissionPage ChromiumPage | ❌ 重定向登录页 |
| headless 检测 | Pydoll 纯异步 CDP | ❌ 被检测 |
| headless 检测 | Camoufox 反指纹 Firefox | ❌ 仍被检测 |
| `_t` 令牌绑定 | Cookie 注入新浏览器 | ❌ 令牌绑指纹 |
| API 签名 | 直接调用搜索 API | ❌ h5st 签名保护 |
| **真实 Chrome CDP 直连** | Playwright connect_over_cdp | ✅ **100% 成功** |

---

## 3. 数据采集结果

### 3.1 京东数据

- **搜索关键词**：iPhone16
- **总页数**：42 页
- **原始数据**：1,260 条商品
- **有效 iPhone 商品**：266 条（经清洗，¥2,500+）
- **自营商品**：10 条（Apple产品京东自营旗舰店）
- **价格区间**：¥2,649 ~ ¥10,999
- **价格中位数**：¥5,168
- **平均价格**：¥5,343

### 3.2 淘宝数据

- **搜索关键词**：iPhone16
- **总页数**：43 页
- **原始数据**：2,022 条商品
- **有效 iPhone 商品**：31 条（经清洗，¥2,500+）
- **价格区间**：¥2,540 ~ ¥6,599
- **价格中位数**：¥3,488
- **平均价格**：¥3,874

### 3.3 数据清洗规则

```python
# 清洗管线
1. 名称过滤：必须包含 16/iPhone/苹果
2. 价格过滤：¥2,500 ~ ¥15,000（排除配件）
3. 配件排除：壳、膜、充电、支架、线、保护
4. 模型分类：Pro Max / Pro / Plus / 标准版
5. 去重：根据标题 + 价格归一化去重
```

---

## 4. 对比分析结论

### 核心发现

```
         京东                    淘宝
    ┌─────────────────┐    ┌─────────────────┐
    │  均价 ¥5,343     │    │  均价 ¥3,874     │
    │  中位 ¥5,168     │    │  中位 ¥3,488     │
    │  区间 2,649-10,999│    │  区间 2,540-6,599│
    │  自营 10 件       │    │  无官方自营       │
    │  国行全新正品为主  │    │  卡贴机/美版/二手 │
    └─────────────────┘    └─────────────────┘
          │                        │
          └──────────┬─────────────┘
                     ▼
          京东均价高出淘宝 38%
          但商品质量更可靠（国行正品）
```

### 价格区间分布

| 价格区间 | 京东 | 淘宝 |
|---------|------|------|
| ¥2,500 - 3,500 | 43 件 | 16 件 |
| ¥3,500 - 4,500 | 62 件 | 8 件 |
| ¥4,500 - 5,500 | 45 件 | 3 件 |
| ¥5,500 - 6,500 | 49 件 | 3 件 |
| ¥6,500 - 8,000 | 49 件 | 1 件 |
| ¥8,000 - 11,000 | 18 件 | 0 件 |

### 各型号均价对比

| 型号 | 京东均价 | 淘宝均价 |
|------|---------|---------|
| iPhone 16 标准版 | ~¥5,000 | ~¥3,500 |
| iPhone 16 Plus | ~¥5,700 | ~¥4,000 |
| iPhone 16 Pro | ~¥6,400 | ~¥5,000 |
| iPhone 16 Pro Max | ~¥7,600 | ~¥6,500 |

### 结论

1. **京东价格真实反映国行正品市场价** — ¥5,000-11,000 区间合理
2. **淘宝价格低但商品质量参差不齐** — 大量为卡贴机/美版/二手/翻新机
3. **同等品质（国行全新正品）下，两平台价差约 10-15%**，非表面上的 38%
4. **淘宝 ¥8,000 以上高端机型几乎为空白**

---

## 5. 技术亮点

### 5.1 独创的 CDP 反反爬方案

**解决了 headless 检测 + 令牌指纹绑定 + SPA 渲染 + API 签名四层反爬**，这套思路可复用于任何国内强反爬站点（京东、淘宝、拼多多、抖音电商等）。

### 5.2 全流程自动化

```
爬取 → 清洗 → 去重 → 分析 → 图表生成
全部脚本化，一键运行
```

### 5.3 数据质量保证

- 双重清洗管线（名称 + 价格 + 配件过滤）
- 标题归一化去重
- 多个价格字段交叉验证

### 5.4 可复用的技能包

建立完整的 `jd-price-scraper` skill：
- 5 层爬取方案（SessionPage → ChromiumPage → Camoufox → CDP → API）
- 4 个参考文档（启动指南、Cookie 注入、比价模板、图表生成）
- 3 个可运行脚本（单页爬取、多页全量爬取、图表生成）

---

## 6. 交付物

| 文件 | 说明 |
|------|------|
| `jd_cdp_scraper.py` | 京东 CDP 单页爬虫脚本 |
| `jd_multi_page_scraper.py` | 京东全量 42 页爬虫脚本 |
| `tb_scraper.py` | 淘宝全量爬虫脚本 |
| `jd_chart_generator.py` | 数据清洗 + Chart.js 图表生成 |
| `jd_vs_tb_comparison.html` | 京东 vs 淘宝对比可视化 |
| `jd_iphone16_price_chart.html` | 京东单站价格分析可视化 |
| `scraping_master.md` | 爬虫数据分析师知识库（12 模块） |

---

## 7. 技术栈速览

```
Web 采集     ████████████████████████████████████░░░  90%
数据清洗     ████████████████████████████████████░░░  90%
反反爬       ███████████████████████████████████████   95%
可视化       ██████████████████████████████████░░░░░  80%
Python       ███████████████████████████████████████   95%
Playwright   ████████████████████████████████████░░░  90%
CDP 协议     ██████████████████████████████████░░░░░  85%
```

---

> **适用客户场景**：
> - 需要监控竞品价格的电商卖家/品牌方
> - 需要中国电商平台数据的研究机构/分析师
> - 需要跨境比价的跨境电商
> - 需要绕过强反爬获取数据的任何项目
