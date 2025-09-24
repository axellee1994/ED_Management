# Emergency Department Load Forecasting

A comprehensive time series forecasting project using SARIMAX modeling to predict Emergency Department (ED) attendance across Singapore's major hospitals for the next 7 days.

## 🎯 Project Objectives

**Primary Goal:** To forecast inpatient admissions arising from ED visits for the next 7 days

**Key Benefits:**
- Enable proactive resource allocation across hospital networks
- Improve patient care through better capacity planning  
- Support healthcare decision-making with data-driven insights

## 📊 Dataset Overview

### Data Sources
- **ED Attendance Data**: [MOH Singapore Healthcare Statistics](https://www.moh.gov.sg/others/resources-and-statistics/healthcare-institution-statistics-attendances-at-emergency-medicine-departments)
- **Public Holidays**: [Singapore Public Holidays 2023-2025](https://www.mom.gov.sg/employment-practices/public-holidays)
- **Research Reference**: [2021 NEDI Singapore Study](https://journals.lww.com/smj/fulltext/9900/2021_nedi_singapore_study__an_updated_inventory_of.157.aspx)

### Dataset Characteristics
- **Time Coverage**: January 1, 2023 to September 6, 2025 (980 daily records)
- **Hospitals Tracked**: 9 major Singapore hospitals
  - Alexandra Hospital (AH)
  - Changi General Hospital (CGH)
  - Khoo Teck Phuat Hospital (KTPH)
  - Ng Teng Fong Hospital (NTFGH)
  - National University Hospital (NUH)
  - Singapore General Hospital (SGH)
  - Sengkang Hospital (SKH)
  - Tan Tock Seng Hospital (TTSH)
  - Woodlands Health (WH)
- **Average Daily System Load**: ~2,455 ED attendances across all hospitals

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
└── requirements.txt
```

## 📈 Key Findings & Insights

### Seasonal Patterns Discovered

**Weekly Patterns:**
- **Monday Effect**: Peak attendance (2,862 avg) - 31% higher than Saturday
- **Weekend Reduction**: Saturday lowest (2,187 avg), Sunday slight recovery
- **Weekday Decline**: Steady decrease from Monday through Friday

**Monthly Patterns:**
- **Summer Peak**: July shows highest volume (2,509 avg)
- **Post-Winter Lull**: March shows lowest volume (2,408 avg)
- **Seasonal Stability**: Only ~4% variation across months

### Hospital Performance Analysis

**Highest Volume Hospitals:**
1. **Changi General (CGH)**: 371.1 avg daily attendances
2. **Tan Tock Seng (TTSH)**: 366.6 avg daily attendances
3. **Sengkang (SKH)**: 348.9 avg daily attendances

**Most Stable Operations:**
- **SGH**: Standard deviation of 34.4 (most consistent)
- **SKH**: Standard deviation of 36.6 (highly reliable)

## 🤖 Machine Learning Pipeline

### Model Development Approach

1. **Baseline SARIMAX Model**: Standard time series forecasting
2. **Tuned SARIMAX Model**: Auto-optimized parameters using pmdarima
3. **Enhanced SARIMAX with Exogenous Variables**: Calendar-based feature engineering

### Feature Engineering

**Exogenous Variables Created:**
- `IsPublicHoliday`: Singapore public holidays
- `IsSchoolHoliday`: School vacation periods  
- `AfterHoliday`: Day following any holiday
- `IsLongWeekend`: Extended weekend periods
- `IsMonday`: Monday effect indicator

### Model Performance

**Final Model Metrics (Rolling Window Validation):**
- **MAE (Mean Absolute Error)**: 105.28 attendances
- **RMSE (Root Mean Squared Error)**: 124.40 attendances  
- **MAPE (Mean Absolute Percentage Error)**: 4.24%

**Model Improvements:**
- **Exogenous Variables Impact**: Significant improvement in forecast accuracy
- **Monday Effect**: Most influential feature (strongest statistical significance)
- **Holiday Impact**: Public holidays and post-holiday surges effectively captured

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

The trained model automatically generates 7-day forecasts and saves results to:
- **CSV Output**: `results/ed_forecast_next7days.csv`
- **Visualization**: Integrated forecast plots in notebook
- **Model Persistence**: Saved model available at `models/sarimax_tuned_exog_model.joblib`

## 📊 Model Validation & Reliability

### Rolling Window Cross-Validation
- **Validation Method**: 365-day training windows with 7-day forecasts
- **Consistency**: Stable error metrics across all time periods
- **Generalization**: Strong performance on unseen data

### Statistical Diagnostics
- **Residual Analysis**: Normally distributed residuals
- **Ljung-Box Test**: No significant autocorrelation in residuals
- **Stationarity**: Confirmed via Augmented Dickey-Fuller test

## 🎯 Business Impact

### Operational Benefits
- **Resource Planning**: 7-day advance capacity planning
- **Staff Scheduling**: Data-driven workforce allocation
- **Patient Flow Management**: Proactive congestion mitigation

### Accuracy Achievements
- **4.24% MAPE**: High precision for healthcare planning
- **105 attendance MAE**: Operationally acceptable error range
- **Stable Performance**: Consistent across different time periods

## 🔮 Future Enhancements

### Potential Improvements
- **External Data Integration**: Weather, disease outbreaks, special events
- **Individual Hospital Models**: Hospital-specific forecasting
- **Real-time Updates**: Automated daily model retraining
- **Advanced Models**: Ensemble methods, neural networks

### Limitations Addressed
- **External Shocks**: Model doesn't account for sudden disruptions (e.g., pandemics)
- **Data Scope**: Limited to calendar-based features
- **Computational Cost**: Rolling validation is resource-intensive

## 📋 Requirements

### System Requirements
- Python 3.10+
- 8GB RAM minimum (16GB recommended for full analysis)
- Jupyter Notebook environment

### Data Requirements
- Daily ED attendance data (CSV format)
- Holiday calendars (public and school)
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
