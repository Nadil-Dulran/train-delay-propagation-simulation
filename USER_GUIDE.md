# User guide: Train-Delay Propagation Simulation

**Open the demo:** [nadil-dulran.github.io/train-delay-propagation-simulation](https://nadil-dulran.github.io/train-delay-propagation-simulation/)

The demo runs entirely in a modern browser. No installation, server, API, or
data upload is required.

## Controls

| Control | What it does | Range | Default |
|---|---|---:|---:|
| Recovery buffer | Amount of delay recovery applied between stations | 0–5 min | 2.0 min |
| Initial delay at Maradana | Common starting delay applied to all five services | 0–10 min | 0 min |
| Disturbance variability | Scales running noise, dwell noise, and regime shocks | 0–2 | 1.0 |
| Monte Carlo replications | Number of simulated operating periods | 100–5,000 | 2,000 |
| Start simulation | Runs the selected scenario | — | — |
| Pause | Pauses the current run | — | — |
| Reset | Restores the default settings and baseline display | — | — |

## Reading the outputs

- **Mean final delay:** mean arrival delay at Panadura across all simulated
  train outcomes, in minutes.
- **Final delay > 10 min:** percentage of simulated outcomes with a final
  arrival delay greater than 10 minutes.
- **95% CI for mean:** normal confidence interval describing Monte Carlo
  sampling uncertainty in the mean.
- **Train outcomes simulated:** number of train-level outcomes. With the
  default settings, this is 10,000 (2,000 replications × 5 trains).
- **Bar chart:** mean final delay for each of the five simulated services.

The confidence intervals describe uncertainty under the illustrative model
assumptions. They are not real-world prediction intervals.

## Suggested experiments

### Experiment 1: recovery buffer

1. Leave the initial delay at 0 minutes.
2. Run the simulation with recovery buffers of 0, 2, and 5 minutes.
3. Compare the mean final delay and the severe-delay percentage.

Expected result: larger recovery buffers reduce propagated delay in this
simplified model.

### Experiment 2: initial delay

1. Set the recovery buffer to 2 minutes.
2. Run the simulation with initial delays of 0, 5, and 10 minutes.
3. Compare the mean final delay and the severe-delay percentage.

Expected result: larger initial delays increase both measures.

## Notebook comparison

The notebook uses the same default scenario and reports approximately:

- Mean final delay: **2.204 minutes**
- 95% confidence interval: **2.146–2.262 minutes**
- Severe-delay probability: **2.7%**

The browser demo uses a deterministic browser-side random-number generator,
while the notebook uses NumPy's generator. Small differences are therefore
normal; the demo is considered consistent when the mean is within 0.10 minutes
and the severe-delay probability is within 1 percentage point.

## Scope and limitations

The demo models only the six stations common to five selected Maradana–
Panadura services. It uses illustrative Markov, disturbance, dwell-time, and
headway assumptions. It does not model signalling blocks, track occupancy,
overtaking, cancellations, rolling-stock constraints or detailed dispatching.
