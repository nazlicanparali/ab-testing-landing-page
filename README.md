# A/B Test Analysis: New vs. Old Landing Page

An end-to-end statistical analysis of a website A/B test. The goal is to decide whether a redesigned landing page converts more visitors than the current one, and to back that decision with sound experiment design rather than a single p-value.

## Business question

Should the company launch the new landing page (`new_page`) in place of the old one (`old_page`)?

## Dataset

[Kaggle - A/B Testing (`ab_data.csv`)](https://www.kaggle.com/datasets/zhangluyuan/ab-testing): one row per page visit, with the assigned group, the page shown, a timestamp and whether the user converted.

| Column | Description |
|---|---|
| `user_id` | Unique user identifier |
| `timestamp` | Time of the visit |
| `group` | `control` or `treatment` |
| `landing_page` | `old_page` or `new_page` |
| `converted` | 1 if the user converted, 0 otherwise |

The data is not included in this repository. Download `ab_data.csv` from Kaggle and place it in the `data/` folder.

## Approach

1. **Data quality checks:** missing values, users shown a page that does not match their assigned group, and duplicate users. Affected rows are removed and the counts are reported.
2. **Experiment design:** hypotheses, significance level, minimum detectable effect (MDE), required sample size and power analysis, plus the smallest effect the actual sample could detect.
3. **Sample ratio mismatch (SRM) check:** a chi-square test on group sizes to confirm the traffic split looks random.
4. **Exploratory analysis:** conversion rates with Wilson confidence intervals, and daily conversion rates to check stability over time.
5. **Hypothesis test:** two-proportion z-test, confidence intervals for the absolute difference and the rate ratio, and a chi-square test as a cross-check.
6. **Decision:** a recommendation that combines statistical significance with practical significance (the confidence interval compared with the MDE).
7. **Limitations and next steps.**

## Results

After cleaning (3,893 rows with mismatched group/page and 1 duplicate user removed), the analysis uses 290,584 users, split almost exactly 50/50 (SRM check p = 0.95).

| | Old page (control) | New page (treatment) |
|---|---|---|
| Users | 145,274 | 145,310 |
| Conversion rate | 12.04% | 11.88% |

- Difference (new - old): **-0.16 percentage points**, 95% CI [-0.39, +0.08], p = 0.19 (two-proportion z-test; the chi-square check gives the same p-value).
- Relative lift: -1.3%, 95% CI for the rate ratio [0.968, 1.007].
- With this sample size the test could detect an absolute lift of about 0.34 points at 80% power, well below the planned MDE of 1 point.
- **Recommendation:** no evidence that the new page converts better, and the confidence interval rules out an improvement as large as the 1-point MDE. Do not launch the new page for conversion reasons; keep the old page unless other considerations (cost, brand) justify the change.

## How to run

```bash
git clone <your-repo-url>
cd ab-testing-landing-page
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# Place ab_data.csv in data/, then:
jupyter notebook ab_test_analysis.ipynb
```

Experiment settings (significance level, power and MDE) are defined in the first code cell of the notebook. Changing them updates every result and conclusion below it.

## Tools

Python, pandas, NumPy, SciPy, statsmodels, matplotlib.

## Project structure

```
ab-testing-landing-page/
├── ab_test_analysis.ipynb
├── data/                  # place ab_data.csv here (not tracked by git)
├── requirements.txt
└── README.md
```
