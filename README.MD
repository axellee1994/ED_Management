# Emergency Department Inpatient Admission Forecasting

A comprehensive time series forecasting project using SARIMAX modeling to predict inpatient admissions arising from Emergency Department (ED) visits across Singapore's major hospitals for the next 7 days.

## 🎯 Project Objectives

**Primary Goal:** To forecast inpatient admissions arising from ED visits for the next 7 days using machine learning and time series analysis.

**Key Benefits:**
- Enable proactive inpatient bed allocation across hospital networks
- Improve patient care through better capacity planning  
- Support healthcare decision-making with data-driven insights
- Optimize resource allocation for inpatient services

## 📊 Dataset Overview

### Data Sources
- **ED Attendance Data**: [MOH Singapore Healthcare Statistics](https://www.moh.gov.sg/others/resources-and-statistics/healthcare-institution-statistics-attendances-at-emergency-medicine-departments)
- **Public Holidays**: [Singapore Public Holidays 2023-2025](https://www.mom.gov.sg/employment-practices/public-holidays)
- **Admission Rate Reference**: [2021 NEDI Singapore Study](https://journals.lww.com/smj/fulltext/9900/2021_nedi_singapore_study__an_updated_inventory_of.157.aspx)
- **Model Framework**: [2024 Healthcare MDPI Paper](https://www.mdpi.com/2227-9032/12/17/1751)

### Dataset Characteristics
- **Time Coverage**: January 1, 2023 to September 6, 2025 (980 daily records)
- **Hospitals Tracked**: 9 major Singapore public hospitals
  - Alexandra Hospital (AH)
  - Changi General Hospital (CGH)
  - Khoo Teck Phuat Hospital (KTPH)
  - Ng Teng Fong Hospital (NTFGH)
  - National University Hospital (NUH)
  - Singapore General Hospital (SGH)
  - Sengkang Hospital (SKH)
  - Tan Tock Seng Hospital (TTSH)
  - Woodlands Health (WH)
- **Average Daily ED Attendance**: ~2,455 visits across all hospitals
- **Average Daily Inpatient Admissions from ED**: ~791 admissions across all hospitals

## 🏥 Hospital Admission Rate Methodology

### Based on 2021 NEDI Singapore Study - ED Visits Leading to Admission

**Public Sector Hospital Distribution (2021):**
- **22% of hospitals**: <20% admission rate  
- **56% of hospitals**: 20-39% admission rate
- **11% of hospitals**: 40-59% admission rate
- **11% of hospitals**: 60-79% admission rate

**Implementation Approach:**
Given the absence of hospital-specific admission rates, we calculate a **system-wide weighted average**:

```
Weighted Average = (0.22 × 15%) + (0.56 × 29.5%) + (0.11 × 49.5%) + (0.11 × 69.5%)
                 = 3.3% + 16.52% + 5.445% + 7.645%  
                 = 32.91%
```

**Uniform Admission Rate Applied:**
- **All Hospitals**: 32.9% admission rate from ED visits
- **Rationale**: Avoids assumptions about individual hospital characteristics
- **Validation**: To be refined with actual SGH BMU data post-October 2024

### System-Wide Metrics
- **Uniform Admission Rate**: 32.9%
- **Average Daily ED Attendance**: ~2,455 visits across all hospitals
- **Average Daily Inpatient Admissions from ED**: ~807 admissions across all hospitals
- **Peak vs. Average**: Admission patterns follow ED attendance seasonality

*Note: Uniform rate methodology ensures transparency and avoids unsupported hospital classifications. Individual hospital rates will be implemented upon receipt of actual data from SGH BMU.*

## 🔧 Technical Implementation

### Environment & Dependencies

```bash
pip install -r requirements.txt
```

**Core Dependencies:**
- `pandas>=2.0.0` - Data manipulation and analysis
- `numpy>=1.24.0` - Numerical computations
- `matplotlib>=3.7.0` & `seaborn>=0.12.0` - Visualization
- `scikit-learn>=1.3.0` - Model evaluation metrics
- `statsmodels>=0.14.0` - SARIMAX time series modeling
- `pmdarima>=2.0.0` - Automated ARIMA parameter selection
- `joblib>=1.3.0` - Model serialization

### Project Structure

```
ED_Management/
├── data/
│   ├── attendance/
│   │   └── Attendances at EMD_week37Y2025.csv
│   └── holidays/
│       ├── singapore_public_holidays_2023_2025.csv
│       └── singapore_school_holidays_2023_2025.csv
├── models/
│   └── sarimax_tuned_exog_model.joblib
├── results/
│   ├── rolling_window_metrics.csv
│   └── ed_forecast_next7days.csv
├── src/
│   └── ED_Management.ipynb
├── pdf/
│   ├── 2021_nedi_singapore_study__an_updated_inventory_of.157.pdf
│   └── healthcare-12-01751.pdf
└── requirements.txt
```

## 📈 Key Findings & Insights

### Seasonal Patterns Discovered

**Weekly Patterns:**
- **Monday Effect**: Peak attendance (2,862 avg) translates to ~923 admissions
- **Weekend Reduction**: Saturday lowest (2,187 avg) translates to ~705 admissions
- **Weekday Decline**: Steady decrease from Monday through Friday
- **Admission Impact**: 31% higher Monday admissions vs Saturday

**Monthly Patterns:**
- **Summer Peak**: July shows highest volume (2,509 avg attendance, ~808 admissions)
- **Post-Winter Lull**: March shows lowest volume (2,408 avg attendance, ~776 admissions)
- **Seasonal Stability**: Only ~4% variation across months for both attendance and admissions

### Hospital Performance Analysis

**Volume Analysis (Uniform 32.9% Admission Rate):**
1. **Changi General (CGH)**: 371.1 avg daily attendances → 122.1 daily admissions
2. **Tan Tock Seng (TTSH)**: 366.6 avg daily attendances → 120.6 daily admissions
3. **Sengkang (SKH)**: 348.9 avg daily attendances → 114.8 daily admissions

**All hospitals apply the same 32.9% conversion rate pending actual data validation.**

## 🤖 Machine Learning Pipeline

### Model Development Approach

1. **Baseline SARIMAX Model**: Standard time series forecasting on ED attendance
2. **Tuned SARIMAX Model**: Auto-optimized parameters using pmdarima
3. **Enhanced SARIMAX with Exogenous Variables**: Calendar-based feature engineering
4. **Admission Conversion**: Hospital-specific conversion from attendance to admissions

### Target Variable Transformation

- **Input Data**: Daily ED attendance by hospital (2,455 avg daily total)
- **Conversion Process**: Apply hospital-specific admission rates from 2021 NEDI study
- **Output Target**: Daily inpatient admissions from ED (791 avg daily total)
- **Validation**: Model forecasts admission numbers, not attendance

### Feature Engineering

**Exogenous Variables Created:**
- `IsPublicHoliday`: Singapore public holidays
- `IsSchoolHoliday`: School vacation periods  
- `AfterHoliday`: Day following any holiday
- `IsLongWeekend`: Extended weekend periods
- `IsMonday`: Monday effect indicator

### Model Performance

**Final Model Metrics (Rolling Window Validation):**
- **MAE (Mean Absolute Error)**: ~34 admissions (converted from 105.28 attendance)
- **RMSE (Root Mean Squared Error)**: ~40 admissions (converted from 124.40 attendance)
- **MAPE (Mean Absolute Percentage Error)**: 4.24%

**Model Improvements:**
- **Exogenous Variables Impact**: Significant improvement in forecast accuracy
- **Monday Effect**: Most influential feature for admission patterns
- **Holiday Impact**: Public holidays and post-holiday admission surges captured
- **Admission Conversion**: Evidence-based hospital-specific conversion rates applied

## 🚀 Usage Instructions

### Running the Analysis

1. **Clone the repository**
```bash
git clone [repository-url]
cd ED_Management
```

2. **Install dependencies**
```bash
pip install -r requirements.txt
```

3. **Launch Jupyter Notebook**
```bash
jupyter notebook src/ED_Management.ipynb
```

4. **Execute cells sequentially** for complete analysis pipeline

### Generating Forecasts

The trained model automatically generates 7-day admission forecasts and saves results to:
- **CSV Output**: `results/ed_forecast_next7days.csv` (inpatient admissions)
- **Visualization**: Integrated admission forecast plots in notebook
- **Model Persistence**: Saved model available at `models/sarimax_tuned_exog_model.joblib`

## 📊 Model Validation & Reliability

### Rolling Window Cross-Validation
- **Validation Method**: 365-day training windows with 7-day forecasts
- **Consistency**: Stable error metrics across all time periods
- **Generalization**: Strong performance on unseen data
- **Target Validation**: Validates admission predictions, not attendance

### Statistical Diagnostics
- **Residual Analysis**: Normally distributed residuals
- **Ljung-Box Test**: No significant autocorrelation in residuals
- **Stationarity**: Confirmed via Augmented Dickey-Fuller test
- **Conversion Validity**: Hospital-specific rates grounded in 2021 NEDI study

## 🎯 Business Impact

### Operational Benefits
- **Inpatient Bed Planning**: 7-day advance admission capacity planning
- **Staff Scheduling**: Data-driven nursing and medical staff allocation
- **Resource Management**: Proactive inpatient service preparation
- **System Coordination**: Multi-hospital admission flow optimization

### Accuracy Achievements
- **4.24% MAPE**: High precision for healthcare inpatient planning
- **~34 admission MAE**: Operationally acceptable error range for bed allocation
- **Stable Performance**: Consistent across different time periods
- **Evidence-Based**: Grounded in 2021 NEDI Singapore Study admission rates

## 🔮 Future Enhancements

### Potential Improvements
- **Real Admission Data**: Integration with actual SGH BMU admission data (post-October 2024)
- **External Data Integration**: Weather, disease outbreaks, special events
- **Individual Hospital Models**: Hospital-specific admission forecasting
- **Real-time Updates**: Automated daily model retraining
- **Advanced Models**: Ensemble methods, neural networks for admission patterns

### Limitations Addressed
- **External Shocks**: Model doesn't account for sudden disruptions (e.g., pandemics)
- **Data Scope**: Currently limited to calendar-based features
- **Conversion Assumptions**: Using NEDI study ranges pending actual data validation
- **Computational Cost**: Rolling validation is resource-intensive

## 📋 Requirements

### System Requirements
- Python 3.10+
- 8GB RAM minimum (16GB recommended for full analysis)
- Jupyter Notebook environment

### Data Requirements
- Daily ED attendance data (CSV format)
- Holiday calendars (public and school)
- Hospital admission rate data (using 2021 NEDI study as baseline)
- Minimum 2 years historical data for robust modeling

## 👥 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/enhancement`)
3. Commit changes (`git commit -am 'Add new feature'`)
4. Push to branch (`git push origin feature/enhancement`)
5. Create Pull Request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

**Note**: This forecasting model is designed for healthcare inpatient admission planning support. Hospital admission rates are based on 2021 NEDI Singapore Study and will be validated with actual SGH BMU data. Always consult with healthcare professionals for critical operational decisions.