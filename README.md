# Monte Carlo Analysis of Train-Delay Propagation

[One-sentence pitch: what this simulation does and the question it answers.]

**[Live demo](https://nadil-dulran.github.io/train-delay-propagation-simulation/)

** · [![Open in Colab](https://colab.research.google.com/drive/1mBkqzn0Sc3l0_lTdz1HM4wy8oEuleFlQ?usp=sharing)]

** · [![Open in Colab Notebook in GitHub](https://colab.research.google.com/github/Nadil-Dulran/train-delay-propagation-simulation/blob/main/notebook/SE4042_IT23161788_Gamage_Notebook.ipynb)]

![Screenshot of the web demo](figures/demo-screenshot.png)

## The problem

Train delays can carry from one station to the next. A late departure, longer-than-usual station stop, or unexpected disruption can affect arrival times further along the route. This project simulates five train services between Maradana and Panadura to explore how these factors may influence delays.

The simulation asks how an initial delay and the time available to recover between stations affect final arrival delays and the chance of a delay exceeding 10 minutes. Understanding these effects can help show why timetable recovery time matters. Because detailed operational records for these services were unavailable, the model uses real scheduled times but illustrative assumptions for random disruptions.

## Quick start

**Notebook:** click the *Open in Colab* badge above, then *Runtime → Run all*. Everything installs and runs in about 1 minutes on a free runtime.

**Web demo:** open the [live demo](https://nadil-dulran.github.io/train-delay-propagation-simulation/), or download this repository and double-click `docs/index.html`.

**Run locally (optional):**

```bash
python -m pip install -r requirements.txt
jupyter notebook notebook/SE4042_IT23161788_Gamage_Notebook.ipynb
```

## Using the demo

Use the sliders to set the **recovery buffer**, **initial delay** at Maradana, **disturbance variability**, and number of **Monte Carlo replications**, then start, pause or reset the simulation. 
The results show the **mean final arrival delay**, the **percentage of outcomes exceeding 10 minutes**, a **95% confidence interval**, and the **number of train outcomes simulated**. A bar chart **compares mean final delays** across the five services(Trains).

Full guide: [USER_GUIDE.md](USER_GUIDE.md).

## Model and mathematics

The simulation processes five trains across six shared stations. At each station, the next arrival delay is calculated as:

$$
d^A_{i,s+1} = \max(0,\ d^D_{i,s} + \epsilon_{i,s} + q_{z_{i,s+1}} - b)
$$

where running-time noise ($\epsilon$) and operating-regime shock ($q$) are random, ($z$) follows a three-state Markov chain, and ($b$) is the recovery buffer. Extra dwell time and a minimum departure headway also affect departure delays; Monte Carlo replications estimate final delays and severe-delay probabilities.

Full derivations, assumptions and parameters: [THEORY.md](THEORY.md).

## Verification

The notebook includes:

- stochastic-matrix validity checks;
- an analytical stationary-distribution check;
- empirical versus analytical transition-probability verification using
  200,000 transitions;
- deterministic limiting-case verification against the supplied timetable; and
- minimum departure-headway verification.

These tests provide the rigorous verification evidence required for **X2**.
The timetable data and official system-wide railway statistics provide real-data
context for **X4**, while the recovery-buffer and initial-delay experiments
with confidence intervals provide sensitivity and uncertainty analysis for
**X5**.

| Verification check | Result |
|---|---|
| Markov transition matrix | All probabilities are non-negative; each row sums to 1 |
| Stationary distribution | Low: 0.4242, Moderate: 0.3485, High: 0.2273 |
| Empirical transition test | Over 200,000 transitions, maximum probability error was 0.0054 (below the 0.015 tolerance) |
| Deterministic timetable test | With disturbances disabled, simulated times matched the supplied timetable |
| Minimum departure headway | All simulated departures met the 3-minute minimum |

These checks verify key mathematical properties and limiting behaviours of the
model implemented in the [notebook](notebook/SE4042_IT23161788_Gamage_Notebook.ipynb).
They support confidence in the implementation.

## Concepts applied

This project applies station-by-station simulation and queueing concepts to model train movements and minimum departure headways. A **Markov chain** represents changing operating conditions, while **Monte Carlo** methods sample running-time disturbances and extra dwell times. Confidence intervals **quantify uncertainty** in the estimated delays, **sensitivity analysis** explores the effects of initial delays and recovery buffers, and **verification checks** test the transition matrix, timetable reproduction and headway constraint.

## Results

Under the default assumptions (2-minute recovery buffer, 0-minute initial
delay, and 2,000 replications), the notebook reports:

- Mean final arrival delay: **2.204 minutes**
- 95% confidence interval for the mean: **2.146–2.262 minutes**
- Probability of a final delay greater than 10 minutes: **2.7%**

The sensitivity experiments show that increasing the recovery buffer reduces
both the mean final delay and severe-delay probability. Increasing the initial
delay at Maradana increases both measures.

| Recovery buffer (min) | Mean final delay (min) | P(final delay > 10 min) |
|---:|---:|---:|
| 0 | 8.590 | 33.1% |
| 1 | 4.738 | 11.8% |
| 2 | 2.202 | 2.8% |
| 3 | 0.944 | 0.3% |
| 5 | 0.106 | 0.0% |

Related plots are available in [`figures/`](figures/).

## Limitations and future work

- The Markov transition probabilities, running-time disturbances, dwell-time distribution and minimum headway are illustrative assumptions, not estimates from route-specific operating records. Results therefore show how the model behaves under these assumptions; they should not be treated as predictions of actual train delays.
- The model includes only six stations shared by the five selected services and simplifies railway operations. It does not represent every stop, signalling, track occupancy, overtaking, cancellations or dispatching decisions.
- With detailed train-level arrival and departure records, future work could estimate and validate the model parameters, include all stops and represent additional operational constraints.

## Key references

1. Sri Lanka Railways, *Administration Report 2023* (2023). [PDF](https://railway.gov.lk/web/images/pdf/admin%20report_2023.pdf)
2. Auditor General’s Department, Sri Lanka, *Annual Report 2024* (2024). [PDF](https://auditorgeneral.gov.lk/web/images/audit-reports/upload/2024/Annual_Report/Annual-Report-2024-English-Web_compressed.pdf)
3. Sri Lanka Railways, “Revised Train Timetable from 2025/12/02 onward (Until Further Notice),” Coastal Line timetable notice (2025). [PDF](https://www.railway.gov.lk/web/images/news/2025-12-02%20all%20eng.pdf)

Full list: [REFERENCES.md](REFERENCES.md).

## AI assistance

AI coding assistants were used during development. The author
reviewed, ran and verified the submitted code and documentation.

## Licence and credits

Code and documentation are released under the [MIT Licence](LICENSE).
Timetables and railway reference material remain subject to their
original source rights and are included for academic analysis.