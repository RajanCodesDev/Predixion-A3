# Assignment 3

## Tasks

### [Task2](Task2) — Dialer Campaign Analytics

Analysis of a debt-collection dialer campaign (20,000 accounts × 70,235 dials, Aug 1–30 2026) covering capacity planning, script comparison, funnel-deterioration diagnosis, dialer scheduling logic, and a management memo with prioritized recommendations.

Contents:

| File | Description |
| ---- | ----------- |
| [`A1.md`](Task2/A1.md) | Capacity planning — expected attempts, line occupancy, and required concurrent lines (675). |
| [`A2.md`](Task2/A2.md) | Script A vs Script B comparison — Script A yields 48 more kept promises. |
| [`A3.md`](Task2/A3.md) | Weekly funnel table (Dialed → Connected → RPC → PTP → Paid); largest deterioration is in the RPC rate. |
| [`A4.pseudocode`](Task2/A4.pseudocode) | `next_account()` dialer selection logic — queueing, calling hours, attempt/cooldown/callback limits, priority = days overdue × pending amount. |
| [`analysis.ipynb`](Task2/analysis.ipynb) | Notebook producing all observed figures quoted in the memo. |
| [`management_memo.md`](Task2/management_memo.md) | Executive memo: connection timing, PTP/keep rates, the B7 × Bihar wrong-number anomaly, and three prioritized recommendations. |
| [`gen_data.py`](Task2/gen_data.py) | Synthetic data generator (`accounts.csv`, `calls.csv`) with a seeded RNG. |
| [`accounts.csv`](Task2/accounts.csv), [`calls.csv`](Task2/calls.csv) | Generated account-level and call-level datasets. |
| [`Assignment_2_Dialer_Campaign_Analytics.pdf`](Task2/Assignment_2_Dialer_Campaign_Analytics.pdf) | Assignment brief / problem statement. |

#### Running the analysis

```bash
cd Task2
python gen_data.py        # regenerate accounts.csv and calls.csv
jupyter nbconvert --to notebook --execute analysis.ipynb
```
