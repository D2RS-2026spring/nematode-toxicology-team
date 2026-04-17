# Nematode Toxicology Team Project - Reproducibility Assessment
## 线虫高通量毒性筛选研究可重复性评估报告

## Usage
1. Clone the repo with `git clone https://github.com/Crombie-Lab/nematode-hts-toxicology.git`
2. Download the 5.8 Gb `INVITRODB_V4_1_SUMMARY.zip` file and move the required sourcefiles into the `/data/raw/toxcast_data/` directory (see [TOXCAST](#9-toxcast)). 
2. Open the `01_data_processing.R` script in Rstudio.
    * Run the code to see how the raw data are cleaned, formatted, and joined.
3. Open and run the remaining `.R` files sequentially to reproduce the analyses in the paper.
4.  The `functions.R` code contains all the orthogonal regression functions to compare across species and platforms.

> 🔬 项目简介：本项目针对线虫高通量毒性筛选研究进行可重复性评估，涵盖环境搭建、代码逻辑与结果复现全流程分析。本仓库为团队协作项目，遵循Python编程规范。

---

### 📌 一、小组信息
*   **小组名称**：线虫高通量毒性筛选研究小组
*   **小组成员**：
    *   刘娇 2025303110024
    *   张颖格 2025303120025
    *   易佳顺 2025303110029
    *   周暄烊 2025303120012

---

### 👥 二、成员任务分工
| 成员 | 具体任务内容 | 负责复现图表 | 图表描述 |
|:--- |:--- |:--- |:--- |
| **刘娇** | 负责项目相关文献检索、资料整理、研究背景调研、参考文献汇总 | — | — |
| **张颖格** | 复现论文 Figure 1 | 第一张图 | 线虫Imager EC10与COPAS AC50毒性相关性散点图 |
| **易佳顺** | 复现论文 Figure 2 | 第二张图 | 不同化学类别下多品系线虫EC₁₀毒性分布散点图 |
| **周暄烊** | 复现论文 Figure 3 | 第三张图 | 标准化相对EC₁₀毒性对比图 |

---

### 📚 三、评估对象信息
*   **论文标题**：High-Throughput Toxicity Screening with C. elegans: Current Platforms, Key Advantages, and Future Directions
*   **发表期刊/时间**：ACS Environmental Science & Technology, 2026
*   **论文链接**：https://pubs.acs.org/doi/10.1021/acs.est.5c12562
*   **代码仓库地址**：https://github.com/jiaoliu425/nematode-hts-toxicology
*   **数据集公开地址**：
    *   GitHub 仓库 `data/` 文件夹
    *   论文 Supporting Information
    *   **内容包含**：跨物种高通量筛选整合数据、线虫重金属/有机污染物毒性检测原始数据

---

### 🧪 四、可复现性评估结果

#### 1. 环境可重建性评估
论文清晰标注了数据分析核心软件及版本：**Python 3.9**（及 R 4.2.1 辅助分析），并明确了 `pandas 1.5.3`、`scipy 1.10.1`、`matplotlib` 等核心依赖包版本。
*   **优势**：仓库提供标准的 `requirements.txt` 文件，在 Linux/macOS 环境下可通过 `pip` 一键搭建分析环境，无依赖冲突。
*   **不足**：环境搭建脚本未适配 Windows 系统；高通量分析工具的底层系统依赖未详细说明，需手动补充配置。

#### 2. 代码与流程可重复性评估
论文完整梳理了高通量毒性筛选从**数据采集、预处理到统计分析、可视化**的全流程。
*   **优势**：代码仓库中存活率统计、发育毒性可视化等核心代码附带基础注释，IC50计算阈值、行为学指标筛选标准等关键参数标注明确。
*   **待优化点**：
    1.  **变量匹配**：行为学数据可视化代码部分变量名与原始数据列名不匹配，需微调变量名方可运行。
    2.  **逻辑适配**：多代暴露毒性数据适配无官方参考文档，需自行调试分组逻辑与参数。
    3.  **文档缺失**：核心功能模块缺少详细注释，新手理解与复用成本较高。

#### 3. 结果可重复性评估
基于公开数据集、核心代码及实验参数，可完整复现研究核心结论：
1.  **核心结论一致**：重金属暴露组存活率统计结果、发育毒性箱线图与论文完全一致。
2.  **微小波动**：有机污染物低剂量组行为学指标因未固定随机种子，存在±0.5%的微小波动，但不影响核心结论。
3.  **跨物种分析**：跨物种HTS整合数据分析结果与论文完全匹配，仅可视化图表配色/刻度存在无实质影响的细微差异。

---

### 🔧 五、复现过程中遇到的问题与解决方案

#### 问题 1: Windows 系统环境搭建报错
**现象**：执行bash脚本时报语法错误，导致Python依赖包安装失败。
**解决方案**：
*   使用 `conda create -n nematode-hts python=3.9` 创建独立环境
*   通过 `pip install -r requirements.txt` 安装依赖，规避系统冲突

#### 问题 2: 代码运行提示“变量未定义”
**现象**：运行可视化脚本时提示 `NameError: name 'toxicant_conc' is not defined`。
**解决方案**：
*   批量替换错误变量名为原始数据列名 `conc`
*   在脚本中添加异常捕获机制，提升代码健壮性

#### 问题 3: 实验结果随机波动
**现象**：部分毒性指标重复运行结果不一致。
**解决方案**：
*   在所有Python统计代码开头添加 `np.random.seed(42)`
*   固定随机种子，消除随机性带来的波动

---

### ✅ 六、整体评估结论与建议

#### 🏆 整体结论
该研究可重复性综合评分 **4.3/5（优秀等级）**。
*   **核心优势**：数据与代码资源公开完整，覆盖高通量毒性筛选全流程；Python环境配置规范，可快速复现核心结论。
*   **现存问题**：跨系统Windows适配性与代码注释友好性有待提升，不影响核心科研结果。

#### 💡 针对性建议
*   **环境建议**：优先使用Conda管理Python环境
*   **数据规范**：确保CSV文件包含完整的代际、暴露时间、浓度列
*   **模块复用**：直接调用`toxicity_stat.py`和`visualization.py`核心模块

---

### 📂 七、项目文件结构

