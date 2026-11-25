# Theory and mathematics

## 1. Problem formulation

The simulation represents five scheduled services travelling from Maradana to
Panadura through six common stations. A service can depart late because of an
initial delay, accumulated running-time disturbance, extra station dwell time,
or the minimum headway required behind the preceding service.

The primary quantities of interest are:

- Final arrival delay at Panadura;
- Mean final arrival delay over Monte Carlo replications; and
- The probability that final arrival delay exceeds 10 minutes.

## 2. Assumptions and parameters

The following are modelling assumptions, not measured train-level parameters:

1. Services are processed in scheduled order.
2. The same initial-delay value is applied to each service in an initial-delay
   sensitivity scenario.
3. Operating conditions follow three latent regimes: Low, Moderate, and High.
4. The regime transition matrix is

   $$
   P =
   \begin{bmatrix}
   0.70 & 0.25 & 0.05 \\
   0.30 & 0.50 & 0.20 \\
   0.10 & 0.30 & 0.60
   \end{bmatrix}
   $$

5. Running-time noise is normally distributed with mean 0 and standard
   deviation 1 minute.
6. Extra dwell time is exponentially distributed with mean 0.8 minutes.
7. Regime shock ranges are 0–0.5 minutes (Low), 0.5–2 minutes (Moderate), and
   2–5 minutes (High).
8. A 3-minute minimum departure headway is enforced.
9. The recovery buffer is subtracted from propagated arrival delay.
10. The destination station has an arrival time only.

| Symbol or parameter | Meaning | Units | Default |
|---|---|---|---|
| $d^D_{i,s}$ | Departure delay for train $i$ at station $s$ | min | scenario-dependent |
| $d^A_{i,s}$ | Arrival delay for train $i$ at station $s$ | min | scenario-dependent |
| $\epsilon_{i,s}$ | Running-time disturbance | min | $N(0,1^2)$ |
| $q_z$ | Regime shock | min | regime range |
| $b$ | Recovery buffer | min | 2 |
| $W_{i,s}$ | Extra dwell time | min | Exponential(mean 0.8) |
| $h$ | Minimum departure headway | min | 3 |
| $n$ | Monte Carlo replications | replications | 2,000 |


## 3. Model equations

For train $i$ travelling from station $s$ to $s+1$, the propagated arrival delay is:

$$
d^A_{i,s+1} = \max\left(0,\ d^D_{i,s} + \epsilon_{i,s} + q_{z_{i,s+1}} - b\right)
$$

The operating regime follows the Markov transition rule:

$$
P(z_{i,s+1}=j \mid z_{i,s}=k) = p_{kj}
$$

At an intermediate station, the additional dwell time is:

$$
W_{i,s} \sim Exp(0.8)
$$

where $0.8$ is the mean dwell time in minutes.

The actual departure time is:

$$
D^{act}_{i,s} =
\max\left(
D^{sch}_{i,s},
A^{act}_{i,s} + W^{sch}_{i,s} + W_{i,s}
\right)
$$

The simplified headway constraint is:

$$
D^{\mathrm{act}}_{i,s}
\geq
D^{\mathrm{act}}_{i-1,s} + h
$$

## 4. Numerical and simulation method

The notebook uses a discrete-time, station-by-station simulation. Each
replication:

1. Processes the five services in timetable order;
2. Samples the next Markov operating regime;
3. Samples running-time and dwell-time disturbances;
4. Propagates delay to the next station;
5. Applies the scheduled departure and headway constraints; and
6. Records each train's final Panadura arrival delay.

The simulation is implemented in Python using NumPy and pandas. A master seed
of `20261028` makes the reported experiments reproducible.

For the mean final delay, the notebook reports the normal 95% Monte Carlo
confidence interval:

$$
\bar{x}\pm1.96\frac{s}{\sqrt{n}}.
$$

For the severe-delay probability, where severe delay means $x>10$ minutes,
the notebook reports a 95% Wilson interval:

$$
\frac{
\hat p+z^2/(2n)
\pm
z\sqrt{\hat p(1-\hat p)/n+z^2/(4n^2)}
}{
1+z^2/n
},
\qquad z=1.96.
$$

## 5. Verification

| Check | Expected result | Obtained result |
|---|---|---|
| Transition matrix validity | Non-negative rows summing to 1 | Passed |
| Analytical stationary distribution | $\pi P=\pi$ | Low 0.4242, Moderate 0.3485, High 0.2273 |
| Empirical transition test | Maximum absolute error below 0.015 using 200,000 transitions | 0.0054; passed |
| Deterministic limiting case | Reproduce supplied timetable exactly | Passed |
| Minimum departure headway | Every departure gap is at least 3 minutes | Passed |

The deterministic limiting-case test is especially important: with random
disturbances, recovery, and headway effects disabled, the implementation
reproduces the timetable input rather than introducing an unexplained offset.

## 6. Sensitivity and uncertainty analysis

Two controlled experiments are included:

1. **Recovery-buffer sensitivity:** Buffers of 0, 1, 2, 3, and 5 minutes are
   compared with zero initial delay.
2. **Initial-delay sensitivity:** Initial delays of 0, 5, and 10 minutes are
   compared while holding the recovery buffer at 2 minutes.

Each scenario uses 2,000 replications and confidence intervals. The results
are conditional on the assumptions above and are not predictions of actual
railway performance.

## 7. Concepts applied

| Concept | Application |
|---|---|
| Markov chain | Represents transitions between illustrative operating regimes |
| Monte Carlo simulation | Estimates delay distributions from repeated random scenarios |
| Input modelling | Uses timetable observations and explicitly documented assumptions |
| Discrete-event / queueing ideas | Processes services in order and enforces headway |
| Confidence intervals | Quantifies Monte Carlo uncertainty |
| Sensitivity analysis | Varies recovery buffer and initial delay |
| Verification | Tests mathematical properties, limiting behaviour, and constraints |

## 8. References

See [REFERENCES.md](REFERENCES.md).