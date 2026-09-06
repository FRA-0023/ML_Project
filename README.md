# ⚖️ Cost-Sensitive Risk Modeling: Ordinal Stratification Under Extreme Imbalance

[![Platform](https://img.shields.io/badge/Platform-KNIME%20Analytics-FFA500?style=flat&logo=knime)](https://www.knime.com/)
[![Methodology](https://img.shields.io/badge/Methodology-Cost--Sensitive%20Learning-blue)](https://en.wikipedia.org/wiki/Cost-sensitive_learning)
[![Risk Strategy](https://img.shields.io/badge/Domain-Behavioral%20Risk%20Stratification-red)](#)

> Machine learning under asymmetric loss functions: preserving 5-class ordinal risk resolution on severely imbalanced student behavioral data.

---

## 📌 Executive Summary

Standard classification models assume symmetric error costs—misclassifying an escalating, high-risk behavioral pattern as benign carries the exact same penalty as a false alarm. In clinical, educational, and credit triage, this assumption produces catastrophic blind spots.

This project investigates whether secondary-school student weekday drinking habits can be reliably stratified using a **cost-sensitive machine learning architecture**. 

Rather than defaulting to arbitrary metric optimization, this research tackles the fundamental engineering tension between **Predictive Stability** and **Semantic Resolution**:
- Exposes why collapsing ordinal classes to inflate accuracy destroys critical risk detection.
- Employs **targeted oversampling** to maintain fidelity across minority high-risk tiers.
- Maps the true contextual drivers of behavioral risk, isolating academic friction from social noise.

The complete automated experimental workflow is packaged in [Knime File.knwf](Knime%20File.knwf).

---

## 🔍 Key Empirical Insights

### 1. The Resolution vs. Stability Trade-Off
A frequent shortcut in behavioral modeling is aggregating ordinal targets into coarse binary or 3-class buckets (e.g., Low, Medium, High). 

| Formulation | Agreement Metric (Accuracy) | Ordinal Resolution | Clinical / Triage Utility |
|---|:---:|:---:|---|
| **3-Class Aggregated** | **Inflated (+14-18%)** | Coarse | Masks early escalation between adjacent risk thresholds. |
| **5-Class Native** | Lower raw accuracy | **High (Full Spectrum)** | Preserves exact risk boundaries; penalizes cross-tier misclassifications. |
| **5-Class + Oversampling** | **Optimized Cost Score** | **High (Full Spectrum)** | **Superior cost-sensitive performance**; prevents severe false negatives. |

*Key finding:* Class aggregation provides the illusion of stability while discarding critical edge variance. Combining 5-class native modeling with class-aware oversampling delivers the highest practical utility under asymmetric penalty matrices.

### 2. Contextual Variance: Academic Drivers vs. Social Noise
Feature subset evaluation reveals a sharp hierarchy in predictive power:
- **Academic Context (Primary Driver):** Past academic failures, absenteeism, and structured study time account for the largest proportion of target variance. Academic strain is the foremost early indicator of habitual consumption.
- **Social & Family Context (Weak Signals):** Family cohabitation status, parental educational background, and romantic relationships provide negligible discriminatory power once academic indicators are accounted for.

---

## 📐 Asymmetric Cost Matrix Formulation

Under an asymmetric loss function (\hat{y}, y)$, an underestimate of severe risk ($\hat{y} \ll y$) incurs penalties scaling non-linearly with ordinal distance:

C(\hat{y}, y) = |\hat{y} - y|^k \cdot \mathbb{I}_{\{\hat{y} < y\}} + |\hat{y} - y| \cdot \mathbb{I}_{\{\hat{y} > y\}}

Where underestimation is penalized with exponent  > 1$, forcing the optimizer to err on the side of caution and prioritize high-risk recall over conservative precision.

---

## 🛠️ Architecture & Reproduction

`
Cost_Sensitive_Risk_Modeling/
├── Knime File.knwf                        # Full end-to-end KNIME analytics workflow
│   ├── Preprocessing & Target Binning     # 3-class vs 5-class target generators
│   ├── Oversampling Engine                # Class-imbalance compensation nodes
│   ├── Cost-Sensitive Matrix Scorer       # Asymmetric loss evaluation
│   └── Model Comparison & CI Plots        # Bootstrapped confidence intervals
└── Project_Report.pdf                     # Executive research report
`

### Execution
1. Open [KNIME Analytics Platform](https://www.knime.com/) (v4.7+ recommended).
2. Import Knime File.knwf via File -> Import KNIME Workflow.
3. Execute the pipeline nodes to reproduce feature importance rankings and model comparison curves.

---

**Author:** Francesco Colombini  
[GitHub Profile](https://github.com/FRA-0023) · [LinkedIn](https://www.linkedin.com/in/francescocolombini/)
