# Market Regime Detection Using ML based approaches

[![View in nbviewer](https://img.shields.io/badge/Jupyter-nbviewer-orange)](https://nbviewer.org/github/Anugrah007/Market-Regime-Detection/blob/main/marketRegime.ipynb)

---

## 📌 Introduction
Financial markets exhibit **different regimes** depending on macroeconomic conditions, investor sentiment, and volatility shocks. These regimes — such as *bull*, *bear*, or *high-volatility* states — significantly affect investment performance.  

The goal of this project is to **detect market regimes** in the **E-Mini S&P 500 Futures (ES=F)** using **unsupervised machine learning and statistical methods**, and then design a **simple trading strategy** based on detected states.

---

## 📊 Dataset
- **Source**: [Yahoo Finance](https://finance.yahoo.com/)  
- **Instrument**: E-Mini S&P 500 Dec 25 Futures (**ES=F**)  
- **Period**: Historical daily close prices  

We compute **daily returns** and rolling statistical features from this dataset for regime detection.

---

## ⚙️ Methodologies

This project explores **three regime detection techniques**:

### 1. **Agglomerative Clustering**
- A **hierarchical clustering** method that merges data points into clusters using linkage distances.  
- The process continues until all points belong to a single cluster or a stopping condition is reached.  
- Useful for exploratory insights, but less effective for predictive regime detection.

---

### 2. **Gaussian Mixture Model (GMM)**
- A **probabilistic model** where each cluster is represented by a Gaussian distribution.  
- Parameters (mean, covariance) are estimated using the **Expectation-Maximization (EM)** algorithm.  
- Captures overlapping regimes and provides regime probabilities.  
- More flexible compared to K-Means.

---

### 3. **Hidden Markov Model (HMM)**
- A **sequential probabilistic model** widely used in finance.  
- Defined by:
  - Initial state probabilities  
  - Transition probabilities  
  - Observation likelihoods  
- Uses algorithms such as **Viterbi** or **Baum-Welch** to estimate hidden states.  
- Particularly powerful for **time-dependent market regimes**.  

---

## 🏗️ Modeling and Implementation

### **Encapsulation in `RegimeDetection` class**
- Created a Python object `RegimeDetection` to organize functions for:
  - Agglomerative Clustering  
  - GMM  
  - HMM  

- The **`initialize_model`** function allows parameterized instantiation of models instead of hardcoding.

---

### **Feed-Forward Training and Out-of-Sample Testing**
To improve robustness:
1. Train model on an **initial training dataset**.  
2. Predict state of the **next observation**.  
3. Retrain model every `k` steps (rolling retraining).  

- **Findings**:  
  - **HMM** performed best, identifying major crash periods such as:  
    - 2008 Global Financial Crisis  
    - COVID-19 Crash (2020)  
  - Post-2022 volatile regime was harder to classify due to short-term high-volatility fluctuations.

---

## 📈 Strategy Implementation

Using the detected regimes, we implement a **basic trading strategy**:

- **Long** when regime = *Normal/Stable*  
- **Short** when regime = *Crash/High Volatility*  

⚠️ Simplifications:
- No transaction costs, slippage, or leverage constraints.  
- Strategy compares **P&L** vs. a **Buy & Hold baseline**.  

---

## 📊 Strategy Results

- For about **two years**, both **HMM-based** and **Buy-and-Hold** strategies behaved similarly as no regime shifts occurred.  
- During the **2008 Financial Crisis**, the HMM strategy opened **short positions**, resulting in profits, while Buy-and-Hold suffered heavy losses.  
- During the **COVID-19 Crash (2020)**, the HMM strategy not only avoided losses but also accumulated profits by shorting.  
- From **2020 to early 2022**, both strategies continued to perform well.  
- Post-2022, the HMM strategy struggled due to **lagged regime detection** and **state prediction sparsity**.  
- Nevertheless, as of **January 2023**, the HMM-based strategy still **outperformed Buy-and-Hold**.  

---

## 📝 Conclusion
In this project, we:  
- Applied **Agglomerative Clustering, GMM, and HMM** to **E-Mini S&P 500 Futures (ES=F)**.  
- Conducted both **in-sample** and **out-of-sample** testing.  
- Found that **HMM** was the most effective for identifying market regime shifts.  
- Built a simple **regime-aware strategy** that outperformed **Buy-and-Hold** from 2006–2023.  

---

## 📂 Project Structure
```
├── marketRegime.ipynb              # Main implementation notebook
├── README.md                       # Documentation (this file)
```

---

## 🚀 How to Run

### Prerequisites
- Python 3.8+
- Install dependencies:
  ```bash
  pip install numpy pandas matplotlib seaborn scikit-learn hmmlearn yfinance
  ```

### Steps
1. Clone this repository:
   ```bash
   git clone https://github.com/Anugrah007/Market-Regime-Detection.git
   cd Market-Regime-Detection
   ```
2. Launch Jupyter:
   ```bash
   jupyter notebook
   ```
3. Open **marketRegime.ipynb** and run all cells.

---

## 🌐 View Online
👉 [Open notebook in nbviewer](https://nbviewer.org/github/Anugrah007/Market-Regime-Detection/blob/main/marketRegime.ipynb)

---

## 🖊️ Author
**Anugrah Singh**  
Master in Quantitative Finance, Rutgers Business School  
