# Emergency Department Inpatient Admission Forecasting

This project leverages SARIMAX time series modeling to forecast inpatient admissions arising from Emergency Department (ED) visits across Singapore’s major hospitals for the next 7 days. The pipeline integrates calendar-based exogenous features and is designed to support hospital resource planning and operational decision-making.

---

## Project Overview

- **Objective:** Forecast daily inpatient admissions from ED visits for the upcoming week.
- **Benefits:** Enables proactive bed allocation, improves patient care, and supports data-driven resource management.

---

## Repository Structure

- `src/ED_Management.ipynb` — Main notebook for data processing, modeling, and deployment.
- `data/attendance/` — Weekly ED attendance data (CSV/XLSX).
- `data/holidays/` — Public and school holidays data.
- `models/` — Saved SARIMAX model.
- `results/` — Forecast outputs and validation metrics.
- `pdf/` — Reference papers.
- `requirements.txt` — Python dependencies.

---

## Data Sources & Methodology

- **ED Attendance:** [MOH Singapore Healthcare Statistics](https://www.moh.gov.sg/others/resources-and-statistics/healthcare-institution-statistics-attendances-at-emergency-medicine-departments)
- **Public Holidays:** [Singapore Public Holidays 2023-2025](https://www.mom.gov.sg/employment-practices/public-holidays)
- **Admission Rate Reference:** [2021 NEDI Singapore Study](https://journals.lww.com/smj/fulltext/9900/2021_nedi_singapore_study__an_updated_inventory_of.157.aspx)

**Admission Rate Calculation:**  
A system-wide weighted average admission rate of **32.9%** is applied to ED attendances, based on published distributions. This will be refined with actual hospital data when available.

---

## Key Features

- **Exogenous Variables:** Incorporates public holidays, school holidays, long weekends, and weekday effects.
- **Rolling Window Validation:** Ensures robust model performance and generalization.
- **Forecast Evaluation:** Supports comparison with actual admissions using standard metrics (MAE, RMSE, MAPE).

---

## Usage

1. **Install dependencies:**
   ```sh
   pip install -r requirements.txt
   ```
2. **Run the notebook:**  
   Open `src/ED_Management.ipynb` in Jupyter or VS Code.
3. **Upload data:**  
   Place attendance and actual admissions files in `data/attendance/`.
4. **Generate forecasts:**  
   The notebook outputs 7-day forecasts and saves results to `results/ed_forecast_next7days.csv`.

---

## Model Performance

- **MAE:** ~34 admissions
- **RMSE:** ~40 admissions
- **MAPE:** 4.24%
- **Monday effect and holidays** are significant predictors of admission surges.

---

## Data Privacy & Collaboration

- Ensure all shared data is anonymized and free of sensitive information.
- For large files, consider using Git LFS or external storage.
- Contributions are welcome via pull requests.

---

## License

This project is licensed under the MIT License.

---

**Note:**  
Admission rates are based on the 2021 NEDI Singapore Study and will be updated with actual hospital data when available. This tool is intended to support, not replace, professional healthcare decision-making.