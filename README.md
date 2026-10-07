# A/B Test Analysis: New vs. Old Landing Page

Analysis of a website A/B test: does a new landing page convert more visitors than the old one?

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

1. Data checks: missing values, users whose page doesn't match their group, duplicate users.
2. Experiment design: hypotheses, α, power, MDE (1 point) and the required sample size.
3. Sample ratio mismatch check (chi-square on the group sizes).
4. Conversion rates with Wilson CIs and daily rates over time.
5. Two-proportion z-test, CIs for the difference and the rate ratio, chi-square as a check.
6. Decision: significance and the CI compared with the MDE.
7. Limitations.

## Results

After cleaning (3,893 rows with mismatched group/page and 1 duplicate user removed), the analysis uses 290,584 users, split almost exactly 50/50 (SRM check p = 0.95).

| | Old page (control) | New page (treatment) |
|---|---|---|
| Users | 145,274 | 145,310 |
| Conversion rate | 12.04% | 11.88% |

- Difference (new - old): **-0.16 percentage points**, 95% CI [-0.39, +0.08], p = 0.19 (two-proportion z-test; the chi-square check gives the same p-value).
- Relative lift: -1.3%, 95% CI for the rate ratio [0.968, 1.007].
- With this sample size the test could detect an absolute lift of about 0.34 points at 80% power, well below the planned MDE of 1 point.
- Recommendation: don't launch the new page for conversion. There is no significant difference, and the upper end of the CI (+0.08 points) is far below the 1-point MDE, so the improvement we were looking for is very unlikely.

## How to run

```bash
git clone https://github.com/nazlicanparali/ab-testing-landing-page.git
cd ab-testing-landing-page
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# Place ab_data.csv in data/, then:
jupyter notebook ab_test_analysis.ipynb
```

α, power and the MDE are set in the first code cell of the notebook.

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
