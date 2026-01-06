# TSRF: Thermodynamic Simulation-assisted Random Forest

> **An Explainable Fault Diagnosis Framework for Marine Diesel Engines**  
> 
> **Paper Title:** *Thermodynamic simulation-assisted random forest: Towards explainable fault diagnosis of combustion chamber components of marine diesel engines*  
> **Journal:** *Measurement* (Elsevier), Vol. 251, 2025  
> **DOI:** [10.1016/j.measurement.2025.117252](https://doi.org/10.1016/j.measurement.2025.117252)

[![DOI](https://img.shields.io/badge/DOI-10.1016%2Fj.measurement.2025.117252-blue)](https://doi.org/10.1016/j.measurement.2025.117252)
[![Status](https://img.shields.io/badge/Status-Published-success)]()
[![Topic](https://img.shields.io/badge/Topic-Explainable%20AI-orange)]()

---

## 📑 目录 (Table of Contents)
- [项目背景 (Background)](#-项目背景-background)
- [核心方法论 (Methodology)](#-核心方法论-methodology)
  - [1. 一维热力学建模与故障仿真](#1-一维热力学建模与故障仿真-thermodynamic-modeling)
  - [2. 基于 SHAP 的特征工程](#2-基于-shap-的特征工程-shap-analysis)
  - [3. 随机森林诊断模型](#3-随机森林诊断模型-random-forest)
- [实验结果与分析 (Results)](#-实验结果与分析-results)
- [案例研究：活塞环磨损 (Case Study)](#-案例研究-case-study)
- [引用本文 (Citation)](#-引用本文-citation)

---

## 🔍 项目背景 (Background)

在船舶动力系统的智能化运维中，**燃烧室组件（Combustion Chamber Components）** 是最关键但也最难监测的部位。传统的故障诊断方法面临两大核心痛点：

1.  **“数据孤岛”与样本稀缺**：
    *   船舶柴油机造价极高，难以在真实运营中通过破坏性实验获取故障数据。
    *   现有的训练数据大多基于正常工况，导致模型对突发故障的泛化能力（Generalizability）极差。

2.  **AI 模型的“黑箱”性质**：
    *   深度学习模型虽然准确率高，但缺乏物理层面的解释。
    *   轮机工程师无法理解模型为何做出特定判断，导致智能诊断系统在实际工程中难以落地信任。

本研究提出的 **TSRF** 框架，通过融合**物理机理模型（Thermodynamic Simulation）**与**可解释性机器学习（XAI）**，成功解决了上述问题。

---

## 🛠 核心方法论 (Methodology)

TSRF 框架由三个核心模块组成：数据生成、特征筛选与解释、故障分类。

### 1. 一维热力学建模与故障仿真 (Thermodynamic Modeling)

我们构建了一个高精度的船舶柴油机一维热力学模型（1D Thermodynamic Model），并通过实验数据进行了严格校准。基于该模型，我们定义了物理参数微调策略，以模拟以下 5 种典型故障：

| 故障代码 | 故障类型 (Fault Type) | 物理参数微调策略 (Parameter Tuning Strategy) | 物理机制简述 |
| :--- | :--- | :--- | :--- |
| **F0** | 正常状态 (Normal) | N/A | 基准状态 |
| **F1** | 气缸盖裂纹 (Head cracking) | $\uparrow$ 气缸盖表面温度 | 裂纹导致散热效率降低，局部热失控 |
| **F2** | 活塞烧蚀 (Piston ablation) | $\uparrow$ 活塞表面温度, $\uparrow$ 漏气量 | 材料剥落破坏密封，导致高温燃气泄漏 |
| **F3** | 气缸套磨损 (Liner wear) | $\uparrow$ 缸径, $\uparrow$ 漏气量 | 磨损间隙增大，导致压缩气体泄漏 |
| **F4** | 活塞环磨损 (Ring wear) | $\uparrow$ 漏气量 | 密封失效，直接导致 Blow-by 现象加剧 |
| **F5** | 活塞环粘着 (Ring sticking) | $\uparrow$ 缸径, $\uparrow$ 活塞温度, $\uparrow$ 漏气量 | 积碳导致环运动受阻，热阻增加，传热恶化 |

> *注：通过这种方式，我们将故障特征与明确的物理参数（如传热系数、几何尺寸）关联起来，实现了“机理驱动的数据增强”。*

### 2. 基于 SHAP 的特征工程 (SHAP Analysis)

面对仿真输出的众多热力学参数，我们引入了 **SHAP (SHapley Additive exPlanations)** 值来定量评估每个参数对故障诊断的边际贡献。

*   **Tree SHAP 算法**：相比于传统的特征选择方法（如 RFE、卡方检验），SHAP 能够捕捉特征间的非线性交互作用。
*   **关键特征发现**：研究发现，**涡轮后排气温度 (P14)**、**漏气热流 (P06)** 和 **气缸套热流 (P05)** 是区分不同故障模式的最关键指标。

### 3. 随机森林诊断模型 (Random Forest)

采用随机森林作为分类器，不仅因其在小样本下具有鲁棒性，更因其树结构天然适配 SHAP 解析。模型输入为经过筛选的热力学参数，输出为具体的故障类别。

---

## 📊 实验结果与分析 (Results)

我们在构建的数据集上对比了不同算法的性能，TSRF 展现了卓越的准确性。

### 性能对比表

| Method | Mean Accuracy | Precision | Recall | Interpretability |
| :--- | :--- | :--- | :--- | :--- |
| KNN | 89.81% | 90.94% | 89.81% | Low |
| SVM | 92.13% | 92.91% | 92.13% | Medium |
| **TSRF (Ours)** | **99.07%** | **94.66%** | **94.44%** | **High** |

*数据来源：Table 8 in original paper.*

---

## 💡 案例研究 (Case Study)

为了展示模型的可解释性，我们以 **活塞环磨损 (F4)** 为例进行深度解析：

1.  **全局视角 (Global Interpretation)**：
    SHAP Summary Plot 显示，对于 F4 故障，**漏气相关参数 (Blow-by)** 的权重显著上升，这与物理事实高度一致。

2.  **局部视角 (Local Interpretation)**：
    对于某个被判定为 F4 的样本，SHAP Waterfall Plot 揭示了具体的决策路径：
    *   **P06 (漏气热流) = 1.641**（显著高于正常值） -> **正向贡献**
    *   **P14 (涡轮后排温)** 出现异常波动 -> **正向贡献**
    
    这种解释能力使得操作人员不仅知道“坏了”，还能根据参数变化反推物理原因，验证诊断的合理性。

---

## 📚 引用本文 (Citation)

本研究为船舶柴油机的智能维护提供了一种**数据与机理融合**的新范式。如果您在研究中使用了我们的方法思路、参数设置或对比数据，请引用以下论文：

**BibTeX:**

```bibtex
@article{Luo2025TSRF,
  title = {Thermodynamic simulation-assisted random forest: Towards explainable fault diagnosis of combustion chamber components of marine diesel engines},
  author = {Congcong Luo and Minghang Zhao and Xuyun Fu and Shisheng Zhong and Song Fu and Kai Zhang and Xiaoxia Yu},
  journal = {Measurement},
  volume = {251},
  pages = {117252},
  year = {2025},
  issn = {0263-2241},
  doi = {10.1016/j.measurement.2025.117252},
  publisher = {Elsevier}
}
