# Tableau Dashboard 设计方案与操作指引

## 一、Dashboard 设计方案

### 整体架构：3个Tab页

| Tab | 名称 | 核心目标 |
|-----|------|---------|
| Tab 1 | 投放总览 Executive Summary | 一眼看清全局KPI和趋势 |
| Tab 2 | 渠道效果对比 Channel Performance | 对比各渠道ROI，找到最优投放组合 |
| Tab 3 | 转化漏斗与优化建议 Funnel & Insights | 展示转化路径，给出优化方向 |

### 视觉风格

- **配色**：深色主题，主色 #000000（黑）+ 强调色 #FF4747（NIO红）/ #FF8C00（橙）
- **字体**：标题用粗体，KPI数字放大加粗突出
- **交互筛选器**：日期范围、渠道（Channel）、广告类型（Campaign Type）
- **每张图表**：必须有标题 + 数据来源标注

---

### Tab 1：投放总览（Executive Summary）

#### 1.1 KPI 卡片（6个）

| KPI | 字段/计算 | 格式 |
|-----|----------|------|
| 总花费 | SUM(spend) | $#,##0 |
| 总曝光量 | SUM(impressions) | #,##0 |
| 总点击量 | SUM(clicks) | #,##0 |
| 总转化量 | SUM(conversions) | #,##0 |
| 平均CTR | AVG(ctr) | 0.00% |
| 平均ROAS | AVG(roas) | 0.00 |

#### 1.2 时间趋势折线图

- X轴：日期（按月）
- Y轴（双轴）：左轴 = 月度花费（面积图），右轴 = CTR / ROAS（折线）
- 颜色区分不同指标

#### 1.3 渠道花费占比饼图

- 维度：channel_used
- 度量：SUM(spend)
- 显示百分比标签

---

### Tab 2：渠道效果对比（Channel Performance）

#### 2.1 渠道核心指标条形图

- 4组并排条形图：CTR / CVR / CPC / CPM
- X轴：channel_used
- 按指标值降序排列，加数据标签

#### 2.2 散点图：花费 vs ROAS

- X轴：SUM(spend)
- Y轴：AVG(roas)
- 气泡大小：SUM(conversions)
- 颜色：channel_used
- 添加参考线：平均ROAS

#### 2.3 热力图：渠道 × 广告类型 的 CTR

- 行：campaign_type
- 列：channel_used
- 颜色深浅：AVG(ctr)
- 显示数值标签

---

### Tab 3：转化漏斗与优化建议（Funnel & Insights）

#### 3.1 转化漏斗图

- 三层漏斗：总曝光 → 总点击 → 总转化
- 每层显示绝对值 + 转化率
- 用横向条形图模拟漏斗效果

#### 3.2 高ROAS vs 低ROAS 对比

- 将campaign按ROAS中位数分为"高效组"和"低效组"
- 对比两组的平均CTR、CVR、CPC、CPA

#### 3.3 数据洞察文字框

- 手动写入3-5条核心Insight
- 放在Dashboard右侧或底部

---

## 二、Tableau Public 操作步骤

### Step 1：导入数据

1. 打开 **Tableau Public Desktop**
2. 左侧"连接" → 点击 **文本文件**
3. 选择 `cleaned_campaign_data.csv`
4. 预览数据，确认字段类型：
   - `date` → 日期
   - `spend`, `impressions`, `clicks`, `conversions` → 数值（度量）
   - `ctr`, `cvr`, `cpc`, `cpm`, `roas`, `cpa` → 数值（度量）
   - `channel_used`, `campaign_type`, `target_audience` 等 → 字符串（维度）
5. 点击 **工作表1** 进入编辑界面

### Step 2：创建计算字段

在"分析"菜单 → "创建计算字段"：

```
// CTR（百分比格式）
CTR_pct = [Ctr] * 100

// 总体CTR（聚合）
Overall_CTR = SUM([Clicks]) / SUM([Impressions])

// 总体CVR（聚合）
Overall_CVR = SUM([Conversions]) / SUM([Clicks])

// 总体ROAS（聚合）
Overall_ROAS = SUM([Revenue]) / SUM([Spend])

// 总体CPA（聚合）
Overall_CPA = SUM([Spend]) / SUM([Conversions])

// ROAS分组
ROAS_Group = IF [Roas] >= 6.0 THEN "高效组" ELSE "低效组" END
```

### Step 3：制作 Tab 1 — 投放总览

#### KPI 卡片（以"总花费"为例，其余类似）
1. 新建工作表，命名"KPI_总花费"
2. 将 `spend` 拖到 **文本** (Text) 标记
3. 右键 → 度量 → **总和**
4. 右键 → 格式 → 数字 → 货币，0位小数
5. 调整字体为 **24pt 加粗**
6. 对其余5个KPI重复此步骤

#### 时间趋势折线图
1. 新建工作表，命名"月度趋势"
2. `date` 拖到 **列** → 右键选择 **月（连续）**
3. `spend` 拖到 **行** → 右键 → 度量 → 总和
4. 在行上再拖入 `ctr` → 右键 → 度量 → 平均值
5. 右键 `ctr` 的轴 → **双轴** → 勾选"同步轴"
6. 标记卡片：spend 选**面积图**，ctr 选**线**
7. 配色：花费用浅灰，CTR用红色

#### 渠道花费饼图
1. 新建工作表，命名"渠道花费占比"
2. `channel_used` 拖到 **颜色**
3. `spend` 拖到 **角度** → 右键 → 度量 → 总和
4. 右上角"智能显示" → 选**饼图**
5. `spend` 再拖到 **标签** → 右键 → "快速表计算" → **合计百分比**
6. 调整标签格式：显示百分比 + 渠道名

### Step 4：制作 Tab 2 — 渠道效果对比

#### 渠道指标条形图
1. 新建工作表，命名"渠道CTR对比"
2. `channel_used` 拖到 **行**
3. `ctr` 拖到 **列** → 右键 → 度量 → 平均值
4. 按 CTR 降序排列（工具栏排序按钮）
5. `ctr` 再拖到 **标签**，格式设为百分比
6. `channel_used` 拖到 **颜色**
7. 对 CVR、CPC、CPM 重复此步骤

#### 散点图
1. 新建工作表，命名"花费vs ROAS"
2. `spend` 拖到 **列** → 度量 → 总和
3. `roas` 拖到 **行** → 度量 → 平均值
4. `channel_used` 拖到 **颜色** 和 **详细信息**
5. `conversions` 拖到 **大小** → 度量 → 总和
6. 标记类型选 **圆**
7. 添加参考线：右键Y轴 → 添加参考线 → 平均值

#### 热力图
1. 新建工作表，命名"渠道×类型CTR"
2. `channel_used` 拖到 **列**
3. `campaign_type` 拖到 **行**
4. `ctr` 拖到 **颜色** → 度量 → 平均值
5. `ctr` 再拖到 **文本/标签**
6. 标记类型选 **方形**
7. 颜色 → 编辑颜色 → 选橙-红渐变色板

### Step 5：制作 Tab 3 — 转化漏斗

#### 漏斗图（用横向条形图模拟）
1. 新建工作表，命名"转化漏斗"
2. 创建参数/维度来表示漏斗层级，或用以下方法：
   - 新建计算字段 `Funnel_Stage`：手动创建三行数据
   - 更简单的方法：用3个独立KPI卡片 + 箭头标注
3. 分别展示：总曝光 → 总点击（CTR=14.04%）→ 总转化（CVR=8.01%）

#### 高效 vs 低效对比
1. 新建工作表
2. `ROAS_Group`（之前创建的计算字段）拖到 **列**
3. `ctr` 拖到 **行** → 度量 → 平均值
4. 同样对比 CVR、CPC、CPA

### Step 6：组装 Dashboard

1. 菜单 → **Dashboard** → **新建 Dashboard**
2. 左侧"大小" → 选择 **自动** 或固定 1200×800
3. 从左侧工作表列表中，逐个拖入已创建的工作表
4. 排版建议：
   - 顶部：6个KPI卡片横排
   - 中部左：趋势图 | 中部右：饼图
   - 底部：条形图 / 热力图
5. 添加筛选器：
   - 在任一工作表中，右键 `channel_used` → **显示筛选器**
   - 右键筛选器 → **应用于** → **使用此数据源的所有工作表**
   - 对 `campaign_type`、`date` 重复操作
6. 创建3个Tab页：重复步骤1-5，每个Tab聚焦不同主题

### Step 7：发布到 Tableau Public

1. 确保已注册 [Tableau Public](https://public.tableau.com/) 账号
2. 菜单 → **服务器** → **Tableau Public** → **保存到 Tableau Public**
3. 登录账号
4. 输入工作簿名称："多渠道广告投放效果分析Dashboard"
5. 勾选要发布的工作表
6. 点击 **保存**
7. 浏览器会自动打开发布后的页面
8. 复制链接即可分享（格式：`https://public.tableau.com/views/...`）
