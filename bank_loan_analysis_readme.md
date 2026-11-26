# 🏦 Bank Loan Analysis — Risk Assessment & Predictive Modeling

![Domain](https://img.shields.io/badge/Domain-Finance-blue)
![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-DuckDB-FFF000?logo=duckdb&logoColor=black)
![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-F2C811?logo=powerbi&logoColor=black)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-ML-F7931E?logo=scikitlearn&logoColor=white)

## 📌 Project Overview
Financial institutions face significant capital risk from loan defaults. This end-to-end data analytics and machine learning project analyzes **38,576 historical bank loan applications** from 2021 across the United States to identify key drivers of loan defaults, predict repayment outcomes, and provide an interactive **Power BI** dashboard for real-time executive risk monitoring.

### 🎯 Primary Objectives
- **Data Cleaning & Feature Engineering:** Standardize raw loan records, handle missing values, and engineer temporal and risk-segmentation features.
- **SQL-Driven KPI Aggregation:** Compute portfolio-level performance metrics and Good vs. Bad loan distributions using DuckDB.
- **Exploratory & Multivariate Analysis:** Uncover high-risk borrower profiles and compounding default factors.
- **Predictive Modeling:** Build classification and regression models to evaluate feature importance and predict total payment outcomes.
- **Interactive BI Dashboarding:** Deliver a multi-page Power BI dashboard enabling scenario filtering and loan-level drill-through.

---

## 📂 Repository Structure
```text
Bank-Loan-Analysis/
│
├── financial_loan.csv                    # Raw historical loan dataset (38,576 rows, 24 columns)
├── Cleaned_Bank_Loan_Data.csv            # Cleaned & feature-engineered dataset (27 columns)
├── Bank_Loan_Analysis.ipynb              # Jupyter Notebook (EDA, DuckDB SQL queries, preprocessing)
├── Bank_Loan_Dashboard.pbix              # Interactive Power BI Dashboard (Summary, Overview, Details)
├── Bank Loan Analysis MDD.pdf            # Method Design Document (architecture & methodology)
├── Bank_Loan_Analysis_Presentation.pptx  # Executive presentation deck
└── README.md                             # Project documentation
```

---

## 📊 Dataset Summary
- **Source:** `financial_loan.csv`
- **Records:** 38,576 individual loan applications (2021)
- **Raw Columns:** 24 → **Engineered Columns:** 27
- **Missing Values:** 1,438 missing entries exclusively in `emp_title` (3.73%), imputed with `'Unknown'`.
- **Target Distribution:** **Good Loans** (`Fully Paid` & `Current`) = **86.18%** | **Bad Loans** (`Charged Off`) = **13.82%**

### Key Engineered Features
| Feature | Description |
| :--- | :--- |
| `issue_month` / `issue_year` | Extracted from standardized `issue_date` (datetime) for temporal trend analysis. |
| `dti_category` | Categorized Debt-to-Income (`dti`) into **Low**, **Medium**, and **High** risk buckets. |
| `is_bad_loan` | Binary target flag mapping `Fully Paid` and `Current` to `0`, and `Charged Off` to `1`. |

---

## 🏗️ System Architecture & Methodology
| Layer | Tools Used | Output / Artifact |
| :--- | :--- | :--- |
| **Ingestion** | Python (`Pandas`) | Raw DataFrame (38,576 × 24) |
| **Cleaning** | Python (`Pandas`) | Imputed `emp_title`, datetime formatting, ID uniqueness check |
| **Feature Engineering** | Python (`Pandas`, `NumPy`) | `issue_month`, `issue_year`, `dti_category`, `is_bad_loan` |
| **EDA & KPIs** | `DuckDB` (SQL), `Seaborn`, `Matplotlib` | KPI summary tables, count plots, correlation heatmap |
| **Modeling** | Python (`Scikit-learn`) | Random Forest Classifier & Linear Regression models |
| **Export** | Python (`Pandas`) | `Cleaned_Bank_Loan_Data.csv` |
| **Dashboarding** | Power BI Desktop | 3-page interactive executive dashboard |

---

## 📈 Portfolio KPIs (SQL & Power BI)
| Metric | Value |
| :--- | :--- |
| **Total Loan Applications** | 38,576 |
| **Total Funded Amount** | $435,757,075 ($435.8M) |
| **Total Amount Received** | $473,070,933 ($473.1M) |
| **Average Interest Rate** | 12.05% |
| **Average Debt-to-Income (DTI)** | 13.33% |

### Good Loans vs. Bad Loans Breakdown
| Loan Category | Status | Applications | % of Portfolio | Total Funded | Total Received |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Good Loan** | Fully Paid & Current | 33,243 | 86.18% | $370.2M | $435.8M |
| **Bad Loan** | Charged Off | 5,333 | 13.82% | $65.5M | $37.3M |

---

## 🔍 Key EDA & Multivariate Insights

### Primary Drivers of Loan Default
1. **Loan Grade:** Risk scales directly with assigned grades. **Grade G** loans default at **31.31%**, compared to **Grade A** loans at **5.69%**.
2. **Loan Purpose:** **Small Business** loans carry the highest default risk (**25.62%**), followed by **Renewable Energy** (**18.08%**) and **House** loans (**15.57%**). Debt Consolidation accounts for the largest application volume (~18K).
3. **Home Ownership:** **Renters** have a higher default rate (**14.57%**) compared to **Mortgage** holders (**12.97%**), reflecting asset-backed financial stability.
4. **Interest Rate & DTI:** Higher interest rates and elevated Debt-to-Income ratios strongly correlate with lower credit grades and higher default probability.

### Compounding Risk Scenarios
| Factor Combination | Avg. Default Rate | Business Implication |
| :--- | :---: | :--- |
| **Grade G + Small Business + High DTI** | **> 45%** | Extreme compounding risk; immediate underwriting red flag. |
| **Grade F/G + Small Business** | **~35%** | Highly toxic asset segment requiring strict vetting. |
| **Renters + High DTI** | **~22%** | Lack of collateral combined with cash-flow strain. |
| **Grade D + Educational** | **~19%** | Unsecured debt with delayed return on investment. |

---

## 🤖 Predictive Modeling Results
Models were trained using an **80/20 train-test split** (`random_state=42`) in Scikit-learn:

| Model | Task | Performance Metric |
| :--- | :--- | :--- |
| **Random Forest Classifier** | Loan Status / Default Prediction | **85.69% Accuracy** |
| **Linear Regression** | Total Payment Prediction | **$R^2$ Score = 0.80** |

### Top Feature Importances (Random Forest Classifier)
1. **DTI (`dti`):** `0.198` — Most critical indicator of borrower financial strain.
2. **Installment (`installment`):** `0.184` — Larger monthly obligations increase charge-off likelihood.
3. **Interest Rate (`int_rate`):** `0.182` — Higher rates push borrowers toward default.
4. **Annual Income (`annual_income`):** `0.174` — Lower income limits financial buffer.

> **Regression Insight:** `int_rate` holds the largest positive regression coefficient (**9,593.09**), showing that incremental increases in interest rates significantly inflate total expected payments over the life of a loan.

---

## 📊 Power BI Dashboard Overview
The Power BI report (`Bank_Loan_Dashboard.pbix`) features three interactive views with dynamic slicers for **Good vs. Bad Loan**, **Grade**, **State**, and **Purpose**:

1. **Summary View:** High-level KPI cards, Good vs. Bad loan donut charts, and a loan-status performance grid.
2. **Overview View:** Monthly application trends, US state-wise choropleth map, loan term breakdown (36 vs. 60 months), employment length, loan purpose, and home ownership treemap.
3. **Details View:** Granular loan-level table with full drill-through support for underwriting audits.

### Scenario Filtering Highlights
- **Best-Case Scenario (`Grade A` + `Mortgage`):** Bad loan rate drops **below 6%**, representing a highly stable investment tier.
- **Worst-Case Scenario (`Grade G` + `Small Business`):** Bad loan rate spikes sharply, pinpointing segments needing immediate lending caps.

---

## 💡 Business Recommendations
| Focus Area | Problem Identified | Recommended Action | Expected Impact |
| :--- | :--- | :--- | :--- |
| **Underwriting** | High default rate in Small Business loans (25.62%) | Cap maximum funding for Small Business loans lacking collateral. | Reduce Bad Loan exposure by **10–15%** in this segment. |
| **Risk Management** | Toxicity in Grades F & G | Tighten DTI eligibility thresholds for Grades F and G approvals. | Prevent high-risk capital allocation and improve portfolio yield. |
| **Marketing** | Strong stability in Grade A loans | Launch targeted promotional campaigns for high-income homeowners. | Grow volume of secure, low-risk Good Loans. |

---

## 🚀 Getting Started

### Prerequisites
- Python 3.x
- Jupyter Notebook or Google Colab
- Power BI Desktop (to open `.pbix`)

### Installation & Usage
1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/bank-loan-analysis.git
   cd bank-loan-analysis
   ```
2. **Install required Python libraries:**
   ```bash
   pip install pandas numpy duckdb scikit-learn matplotlib seaborn
   ```
3. **Run the Notebook:**
   Open `Bank_Loan_Analysis.ipynb` in Jupyter Notebook or Google Colab, ensure `financial_loan.csv` is in the working directory, and run all cells to reproduce the EDA, SQL KPIs, and generate `Cleaned_Bank_Loan_Data.csv`.
4. **Explore the Dashboard:**
   Open the `.pbix` file in **Power BI Desktop** to interact with the Summary, Overview, and Details views.

---

## ⚠️ Limitations & Future Enhancements
- **Current Limitations:** Static 2021 snapshot (does not account for macroeconomic shifts like inflation or interest rate hikes) and potential bias in self-reported `annual_income`.
- **Future Work:**
  - Integrate live credit bureau APIs for real-time DTI and credit score updates.
  - Upgrade modeling to gradient boosting algorithms (**XGBoost** / **LightGBM**) to push accuracy beyond 85%.
  - Incorporate macroeconomic indicators and automated risk-alerting pipelines.

---

## 👤 Author
**Ankush Saini**  
*Data Analysis Intern at SYNTECXHUB*  
Tools: Python (Pandas, Scikit-learn, Seaborn) | SQL (DuckDB) | Power BI