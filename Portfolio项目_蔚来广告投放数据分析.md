# Portfolio项目 Prompt — 蔚来广告投放数据分析Dashboard

## 背景说明（给AI读的上下文）

我是纽约大学数学与计算机科学专业大四学生，正在申请**蔚来（NIO）数据分析实习生（广告投放方向）**。

岗位职责要求：
- 使用Tableau/Power BI/MySQL等BI工具搭建并优化广告投放报表和可视化Dashboard
- 产出数据洞察和分析报告，给出内容优化、投放优化、数据增长建议
- 岗位**明确要求附上过往数据可视化作品链接**

我的目标是在这次session里完成一个可以直接投递的Portfolio项目，最终产出一个发布在**Tableau Public**上的可分享链接，以及一份配套的数据分析报告（Jupyter Notebook或Markdown）。

---

## 项目目标

**项目名称**：多渠道广告投放效果分析 Dashboard（模拟新能源汽车品牌场景）

**最终交付物**：
1. 一个发布在Tableau Public的交互式Dashboard（可分享链接）
2. 一个Python数据清洗+EDA的Jupyter Notebook（展示数据处理能力）
3. 一份1-2页的数据洞察报告（Markdown或PDF），模拟真实业务分析输出

---

## 数据集

使用Kaggle上的 **Marketing Campaign Performance Dataset**：
- URL: https://www.kaggle.com/datasets/manishabhatt22/marketing-campaign-performance-dataset
- 包含字段：campaign type、channel、impressions、clicks、CTR、CPC、conversions、conversion rate、ROAS、spend等

如果这个数据集不够用，可以补充以下任一：
- https://www.kaggle.com/datasets/arashnic/ctr-in-advertisement
- https://www.kaggle.com/datasets/aashwinkumar/ppc-campaign-performance-data

---

## Step 1：数据准备（Python + Pandas）

请帮我完成以下工作，生成一个 `data_prep.ipynb` Jupyter Notebook：

1. **下载并加载数据**
   - 读取CSV文件，打印基本信息（shape、dtypes、head、describe）
   - 检查缺失值、重复值，并处理

2. **数据清洗**
   - 标准化列名（小写+下划线）
   - 处理异常值（如CTR > 100%、负数spend等）
   - 日期列转换为datetime格式（如有）
   - 类别字段统一编码（如channel、campaign_type）

3. **新增计算字段**（如原始数据没有，请计算）：
   - `CTR` = clicks / impressions
   - `CVR` = conversions / clicks（转化率）
   - `CPC` = spend / clicks（每次点击成本）
   - `CPM` = spend / impressions * 1000（每千次展示成本）
   - `ROAS` = revenue / spend（广告支出回报率，如有revenue字段）
   - `CPA` = spend / conversions（每次转化成本）

4. **基础EDA**
   - 各渠道（channel）的CTR、CVR、ROAS分布
   - 各广告类型（campaign_type）的效果对比
   - 时间趋势分析（如有日期）
   - 相关性热力图

5. **导出干净数据**
   - 输出 `cleaned_campaign_data.csv` 供Tableau使用

---

## Step 2：Tableau Dashboard设计规划

请根据数据内容，给我一个**具体的Dashboard设计方案**，包含：

### Dashboard结构（建议分3个Tab/Sheet）

**Tab 1：投放总览（Executive Summary）**
- KPI卡片：总花费、总曝光量、总点击量、总转化量、平均CTR、平均ROAS
- 时间趋势折线图：CTR / ROAS / 花费 随时间变化
- 渠道花费饼图/占比

**Tab 2：渠道效果对比（Channel Performance）**
- 各渠道CTR、CVR、CPC、CPM条形图对比
- 散点图：花费 vs ROAS（气泡大小=转化量）
- 热力图：渠道 × 广告类型 的CTR表现

**Tab 3：转化漏斗与优化建议（Funnel & Insights）**
- 转化漏斗图：曝光 → 点击 → 转化
- 高ROAS vs 低ROAS campaign的特征对比
- 数据洞察文字框（手动写入的Insight）

### 视觉风格要求
- 配色：深色主题或NIO品牌色（黑+红/橙），专业感强
- 字体清晰，KPI数字大且突出
- 每个图表都要有标题和数据来源标注
- 加入交互筛选器：日期范围、渠道、广告类型

---

## Step 3：Tableau操作指引

请给我一份**详细的Tableau Public操作步骤**，包括：

1. 如何将 `cleaned_campaign_data.csv` 导入Tableau Public（Desktop版）
2. 如何创建上述每一个图表的具体步骤（drag哪个字段到哪里、用什么图表类型）
3. 如何设置计算字段（在Tableau里写CTR、ROAS等公式）
4. 如何创建Dashboard并排版
5. 如何发布到Tableau Public并获取分享链接

---

## Step 4：数据洞察报告

请帮我生成一份 `insight_report.md`，模拟真实业务分析输出，内容包括：

**标题**：多渠道广告投放效果分析报告

**结构**：
1. 执行摘要（3-5句话，最重要的发现）
2. 数据概览（时间范围、数据量、覆盖渠道）
3. 核心发现（3-5个具体洞察，每条要有数据支撑）
   - 例："SEM渠道CTR均值为X%，显著高于Display渠道的Y%，建议加大SEM预算比重"
   - 例："周末时段CPM下降约Z%，但转化率无明显差异，存在低成本投放窗口"
4. 优化建议（3条可执行建议，对应JD里的"投放优化、内容优化、数据增长"）
5. 数据局限性说明（诚实说明数据是模拟/公开数据集，非真实投放数据）

---

## Step 5：简历附件包装

最终产出物整理完毕后，请帮我起草一段**简历附注文字**（20字以内），以及一个**GitHub README.md**，包含：
- 项目背景
- 使用工具（Python、Tableau Public）
- 数据来源
- Dashboard链接（预留位置）
- 主要分析结论

---

## 技术要求与约束

- Python环境：请使用标准库（pandas、numpy、matplotlib、seaborn）
- Jupyter Notebook格式，cell有清晰注释
- 所有图表标题和标签用**中文**（面向中国市场岗位）
- Tableau使用**Tableau Public**（免费版），不需要付费功能
- 数据是公开Kaggle数据集，可以在简历/作品集中使用

---

## 优先级

如果时间有限，按以下顺序完成：
1. ✅ `data_prep.ipynb`（Python清洗+EDA，最容易展示技术能力）
2. ✅ Tableau Dashboard设计方案+操作指引（核心交付物）
3. ✅ `insight_report.md`（体现业务分析思维）
4. ⬜ GitHub README（加分项）

---

## 开始

请先确认你理解了这个项目的目标，然后从**Step 1（数据准备）**开始执行。
