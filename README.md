# 🚢 Marine Engine Fault Diagnosis (TSRF)
> **不仅要知道“坏了”，还要知道“为什么”。**  
> 一个结合 **一维热力学仿真 (Thermodynamic Simulation)** 与 **可解释性机器学习 (Explainable AI)** 的船舶柴油机故障诊断框架。

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Paper](https://img.shields.io/badge/Paper-Measurement-orange.svg)](https://doi.org/10.1016/j.measurement.2025.117252)

## 🧐 痛点：为什么 AI 修船这么难？

在船舶工程领域，应用 AI 进行故障诊断一直面临两个“地狱级”难度的问题：

1.  **数据极度匮乏（Small Data）：** 船用柴油机造价昂贵，我们不可能为了训练 AI，故意把发动机砸坏（比如故意让气缸盖裂开）来收集故障数据。没有数据，AI 就是无米之炊。
2.  **AI 是个黑盒（Black Box）：** 深度学习模型可能会告诉你“气缸磨损了”，但无法告诉你“为什么”。对于船员和轮机长来说，**无法解释的诊断结果是不可信的**，也无法指导具体的维修工作。

## 💡 我们的解决方案：TSRF 框架

本项目提出了一种名为 **TSRF (Thermodynamic Simulation-assisted Random Forest)** 的新方法。简单来说，就是 **“用物理模型生成数据 + 用机器学习诊断 + 用 SHAP 讲道理”**。

### 1. 给发动机造个“数字孪生” (Physics-based Modeling)
既然真实故障数据难获取，我们就用 **AVL Boost / 一维热力学模型** 来生成！
我们构建了高精度的柴油机模型，并通过微调关键参数，精准模拟了燃烧室组件的五种常见故障：

*   🧊 **F1:** 气缸盖裂纹 (Head cracking)
*   🔥 **F2:** 活塞烧蚀 (Piston ablation)
*   📏 **F3:** 气缸套磨损 (Liner wear)
*   💍 **F4:** 活塞环磨损 (Ring wear)
*   🧱 **F5:** 活塞环粘着 (Ring sticking)

> **核心优势：** 通过物理仿真，我们将原本稀缺的故障样本变成了可控生成的“富矿”，完美解决了数据不足的问题。

### 2. AI 也能“说人话” (Explainable AI with SHAP)
这是本项目的最大亮点。我们没有盲目堆砌深度学习层数，而是选择了 **随机森林 (Random Forest)** 结合 **SHAP (SHapley Additive exPlanations)** 值。

*   **特征筛选：** 面对几十个传感器数据，SHAP 告诉我们要关注 **涡轮后排气温度 (P14)**、**漏气热流 (P06)** 等关键指标。
*   **决策透明化：**
    *   **微观视角：** 对于某一个具体的故障样本，模型会解释：“判断为活塞环磨损，主要因为漏气热流异常升高，同时排气温度降低。”
    *   **宏观视角：** 全局展示不同参数如何影响各种故障类别的判定逻辑。

## 📊 性能表现

在构建的故障数据集上，本框架实现了 **99.07%** 的平均准确率，且在可解释性上远超传统的 CNN 或 SVM 方法。

| Model | Accuracy | Explainability |
| :--- | :--- | :--- |
| KNN | 89.81% | Low |
| SVM | 92.13% | Low |
| **TSRF (Ours)** | **99.07%** | **High (via SHAP)** |

## 📚 引用与论文 (Citation)

本项目是论文 **"Thermodynamic simulation-assisted random forest: Towards explainable fault diagnosis of combustion chamber components of marine diesel engines"** 的核心实现思路展示。

如果您觉得这个思路对您的研究或工程项目有启发，请考虑引用我们的论文，支持开源研究！🌟

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
