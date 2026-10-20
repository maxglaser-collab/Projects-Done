# Finance & Data Projects — Max Glaser

Projects from my Finance & Data Analysis studies at Reichman University. For my main build, see [hushtick.com](https://hushtick.com).

| Project | File | Tools |
|---|---|---|
| [Personal Expense Tracker](#personal-expense-tracker) | `expense_tracker.ipynb` | Python, Pandas, live FX API, LLM |
| [TASE Event Study](#geopolitical-shocks--tase-hotel-stocks--an-event-study) | `TASE_event_study.xlsx` | Excel, OLS regression |

---

## Personal Expense Tracker

A Python app that tracks and reconciles personal expenses across currencies, converts everything to EUR using live exchange rates, and uses an LLM to generate spending recommendations.

- **Live FX conversion:** pulls current exchange rates from [API name] instead of hard-coded rates.
- **Reconciliation and reports:** cleans and categorizes transactions, then summarizes spending by category and period.
- **AI recommendations:** sends the spending summary to [model/API] and returns practical suggestions.

**Run it:** open `expense_tracker.ipynb` in Colab. Add your own API keys in Colab's Secrets panel (the key icon). No keys are stored in this repo.

---

## Geopolitical Shocks & TASE Hotel Stocks — An Event Study

How do Israeli hotel stocks react when the security situation escalates or calms down? This project measures the abnormal returns of three TASE-listed hotel companies around 12 geopolitical events (2023–2025), and tests whether the market prices this news efficiently.

*Quantitative Finance course project, Reichman University (Arison School of Business), 2026. Team project. [Add team members' names if they agree.]*

### Research question

Do escalation events (e.g. the 7 October Hamas attack, Iranian missile strikes) and de-escalation events (e.g. ceasefires, hostage releases) produce measurable abnormal returns for TASE hospitality stocks? And is the reaction consistent with the semi-strong form of the Efficient Market Hypothesis (EMH)?

### Data

| | |
|---|---|
| **Stocks** | Fattal Hotels, Isrotel, Dan Hotels |
| **Benchmark** | TA-125 Index |
| **Events** | 12 (6 escalation, 6 de-escalation) |
| **Observations** | 36 (3 stocks × 12 events) |
| **Event window** | [−10, +10] trading days (21 days) |
| **Source** | [Add price data source, e.g. TASE / Investing.com] |

### Method

1. **Market model.** For each stock and event, estimated alpha and beta against the TA-125 over a pre-event window, then calculated expected returns.
2. **Abnormal returns.** AR = actual return − expected return. Aggregated into CAR (per stock-event), AAR (average across stocks per day) and CAAR (cumulative average), split by escalation vs. de-escalation.
3. **Hypothesis testing.** t-tests on event-day AAR (H₀: the event has no effect on returns).
4. **Cross-sectional regression.** Regressed each stock-event's 21-day CAR on:
   - **Severity**: a signed −5 to +5 scale, so one coefficient captures both the direction and the size of the event
   - **Pre-event Beta**: market sensitivity
   - **Pre-event Volatility**: stock-specific risk (Model 2 only)
5. **Diagnostics.** VIF for multicollinearity, Q-Q plots for normality of CAR and Beta, and a partial F-test comparing the two models.

### Key findings

**1. The direction is clear.** By day +10, de-escalation events lifted hotel stocks by an average **+2.71%** abnormal return, while escalation events pushed them down **−1.71%**, a gap of about 4.4 percentage points. Both groups move together before the event and only split around day −1.

**2. But it isn't statistically significant at 5%.** On the event day, the escalation reaction had t = −1.74 (significant at 10%, not 5%). The de-escalation reaction had t = 0.29 (not significant). Bad news showed up on day 0, while good news built up more gradually.

**3. The regression has low explanatory power, which is normal for event studies.**

| | Model 1 (Severity + Beta) | Model 2 (+ Volatility) |
|---|---|---|
| R² | 0.092 | 0.145 |
| Adjusted R² | 0.037 | 0.065 |
| Significance F | 0.204 | 0.166 |
| Severity coefficient (p) | +0.0066 (p = 0.084) | +0.0067 (p = 0.076) |
| Beta p-value | 0.779 | 0.449 |
| Volatility p-value | n/a | 0.169 |

Severity has the expected sign and is close to significant, but neither model is significant overall with only 36 observations. Adding Volatility doesn't improve the model significantly (partial F-test p = 0.169), so Model 1 is preferred. All VIFs are ≈ 1.0 (max ≈ 1.15), so there's no multicollinearity.

**4. Over the long run, firm differences matter more than events.** Over the full sample, all three stocks underperformed the TA-125 (+149.5%): Fattal +121.7%, Isrotel +114.1%, Dan Hotels +16.4%. That gap is too large to come from event reactions, so it reflects company-level differences.

**5. A model quirk worth knowing.** On the first trading day after 7 October 2023, Fattal fell −8.51%, but its day-0 abnormal return was *positive* (+0.85%). Because of Fattal's high beta (1.40), the market model expected an even bigger drop. This isn't investors welcoming the war; it shows how beta adjustment works when the whole market crashes.

### Conclusion

The results support the semi-strong EMH: the market reacts in the expected direction, but quickly enough that there's no reliable profit in trading hotel stocks on geopolitical headlines. For investors, holding stronger, more diversified operators did better than trying to time events.

**Limitations:** small sample (36 observations, 6 events per group), a subjective severity scale, and only three stocks in one sector.

### Files

| File | Contents |
|---|---|
| `[filename].xlsx` | Full analysis: returns, market model, AR/CAR/CAAR, regressions, VIF and normality tests |
| `[filename].pdf` | Presentation / written report |

### Tools

Excel (LINEST, Data Analysis ToolPak regression, charts).
