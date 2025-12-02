# Heart Rate Variability Analysis in Congestive Heart Failure

Analysis of autonomic dysfunction patterns in heart failure patients using time-domain HRV metrics and unsupervised learning to identify distinct physiological phenotypes.

## Overview

This project analyzes RR interval data from 29 congestive heart failure patients to characterise autonomic nervous system dysfunction and identify patient subgroups based on heart rate variability patterns. Using signal processing techniques and dimensionality reduction, the analysis reveals significant heterogeneity in autonomic function despite similar clinical diagnoses.

## Dataset

**Source:** PhysioNet Congestive Heart Failure RR Interval Database  
**Patients:** 29 individuals with heart failure (NYHA class I-III)  
**Recording Duration:** 20 hours per patient  
**Sampling Rate:** 250 Hz  
**Data Format:** R-peak annotations from long-term ECG recordings

[Database URL](https://physionet.org/content/chf2db/1.0.0/)

## Methods

### Signal Processing
- **RR Interval Extraction:** Calculated from R-peak annotations using wfdb library
- **Artifact Removal:** Two-step cleaning process
  - Range filtering: Removed intervals <300 ms or >2000 ms (physiologically implausible)
  - Sudden jump detection: Filtered beat-to-beat changes >20% (detection artifacts)
- **Artifact Rate:** ~19% of beats removed (typical for long-duration CHF recordings)

### HRV Metrics Computed
- **SDNN** (Standard Deviation of NN intervals): Overall HRV, reflects total autonomic function
- **RMSSD** (Root Mean Square of Successive Differences): Beat-to-beat variability, reflects parasympathetic activity
- **Mean RR Interval:** Average time between heartbeats
- **Mean Heart Rate:** Converted from mean RR interval

### Visualization Techniques
- **Comparative bar charts:** SDNN and RMSSD distribution across patients with normal thresholds
- **Poincaré plots:** Beat-to-beat dynamics showing RR(n) vs RR(n+1) for representative patients
- **t-SNE dimensionality reduction:** 2D embedding of 4 HRV features to identify phenotypes

### Unsupervised Learning
- **Algorithm:** t-distributed Stochastic Neighbor Embedding (t-SNE)
- **Features:** mean_rr_ms, sdnn_ms, rmssd_ms, mean_hr_bpm
- **Preprocessing:** StandardScaler normalization
- **Parameters:** perplexity=5 (appropriate for n=29), random_state=42

## Key Findings

### Universal Autonomic Dysfunction
- **Mean heart rate:** 167 bpm (normal resting: 60-100 bpm)
- **All patients tachycardic:** Range 135-194 bpm
- **Severely reduced HRV:** All patients below normal thresholds
  - Normal SDNN: >100 ms | Observed range: 7.7-94.0 ms
  - Normal RMSSD: >40 ms | Observed range: 5.4-23.1 ms

### Extreme Heterogeneity
- **12-fold variation in SDNN** despite similar diagnoses
- **4-fold variation in RMSSD** across the cohort
- Suggests different disease stages, etiologies, or medication responses

### Three Distinct Phenotypes (t-SNE Clustering)
1. **Severe Dysfunction Cluster** (RMSSD 5-10 ms, HR 185-195 bpm)
   - Near-complete parasympathetic failure
   - Patients: chf203, 205, 206, 209, 210, 220

2. **Moderate Dysfunction Cluster** (RMSSD 10-15 ms, HR 160-185 bpm)
   - Majority of patients
   - Substantial autonomic impairment

3. **Relative Preservation Cluster** (RMSSD 15-23 ms, HR 135-170 bpm)
   - Better autonomic function (though still abnormal)
   - Patients: chf217, 213, 221, 219

### Clinical Implications
- HRV phenotypes capture physiological variation not reflected in NYHA classification
- Objective thresholds (RMSSD <8 ms) could guide clinical decisions
- Different phenotypes may require tailored therapeutic approaches
- Potential for risk stratification beyond symptom-based assessment

## Repository Structure

```
heart-failure-hrv-analysis/
├── heart_failure_hrv_analysis.ipynb    # Main analysis notebook
├── README.md                            # This file
```

## Requirements

```
wfdb>=4.0.0
numpy>=1.24.0
pandas>=2.0.0
matplotlib>=3.7.0
seaborn>=0.12.0
scikit-learn>=1.3.0
```

Install dependencies:
```bash
pip install wfdb numpy pandas matplotlib seaborn scikit-learn
```

## Usage

1. **Clone the repository**
```bash
git clone https://github.com/leee9-9/heart-failure-hrv-analysis.git
cd heart-failure-hrv-analysis
```

2. **Download the dataset** (if not included)
   - Visit [PhysioNet CHF Database](https://physionet.org/content/chf2db/1.0.0/)
   - Download the complete database as zip file
   - Place in the project root directory

3. **Run the notebook**
```bash
jupyter notebook heart_failure_hrv_analysis.ipynb
```

The notebook will:
- Extract data from the zip file
- Load patient demographics
- Process RR intervals with artifact removal
- Calculate HRV metrics for all patients
- Generate visualisations
- Perform t-SNE phenotype discovery

## Visualizations

### HRV Comparison Panel
Four-panel figure showing:
- SDNN distribution (overall HRV)
- RMSSD distribution (parasympathetic function)
- Mean heart rate distribution
- SDNN vs RMSSD relationship

### Poincaré Plots
Beat-to-beat dynamics for representative patients showing:
- Tight clusters = low variability = severe dysfunction
- Wide clouds = higher variability = better autonomic function

### t-SNE Phenotype Map
Two complementary views:
- Colored by RMSSD (parasympathetic gradient)
- Colored by heart rate (tachycardia patterns)

## Technical Notes

**Strengths:**
- Long-duration recordings (20 hours) capture circadian variation
- Systematic artifact removal ensures data quality
- Multiple complementary visualisation approaches
- Clinical interpretation grounded in cardiovascular physiology

**Limitations:**
- Time-domain metrics only (no frequency-domain analysis)
- Small sample size (n=29) limits statistical power
- Single time point (no longitudinal tracking)
- Medication effects not accounted for

## Skills Demonstrated

- **Signal Processing:** RR interval extraction from ECG annotations, artefact detection and removal
- **Time-Series Analysis:** HRV metric computation, beat-to-beat variability assessment
- **Unsupervised Learning:** t-SNE dimensionality reduction, phenotype discovery
- **Data Visualization:** Multi-panel figures, Poincaré plots, colour-coded scatter plots
- **Clinical Domain Knowledge:** Cardiovascular physiology, autonomic function interpretation, heart failure pathophysiology

## Future Work

- Add frequency-domain HRV analysis (LF, HF, LF/HF ratio)
- Implement non-linear metrics (DFA, sample entropy)
- Correlate HRV phenotypes with clinical outcomes
- Analyze medication effects on autonomic function
- Develop risk prediction models combining HRV with clinical variables

## References

**Dataset Citation:**  
Goldberger, A., Amaral, L., Glass, L., Hausdorff, J., Ivanov, P. C., Mark, R., ... & Stanley, H. E. (2000). PhysioBank, PhysioToolkit, and PhysioNet: Components of a new research resource for complex physiologic signals. *Circulation*, 101(23), e215-e220.

**HRV Standards:**  
Task Force of the European Society of Cardiology and the North American Society of Pacing and Electrophysiology. (1996). Heart rate variability: standards of measurement, physiological interpretation and clinical use. *Circulation*, 93(5), 1043-1065.

## Clinical Context

This analysis leverages clinical expertise in cardiovascular physiology and rehabilitation to interpret autonomic dysfunction patterns in heart failure. The integration of signal processing, machine learning, and domain knowledge demonstrates the value of interdisciplinary approaches in healthcare data science.

## License

This project uses publicly available data from PhysioNet. Please cite the original database when using this analysis.

## Contact

**GitHub:** [@leee9-9](https://github.com/leee9-9)  
**LinkedIn:** [www.linkedin.com/in/leena-nandedkar-635123218]

---

*Analysis completed as part of a health data science portfolio demonstrating signal processing, time-series analysis, and unsupervised learning capabilities in a clinical context.*
