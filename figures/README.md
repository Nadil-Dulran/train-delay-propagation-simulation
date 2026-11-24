# Figures

This folder contains figures produced by the train-delay propagation
simulation. The error bars show 95% confidence intervals.

## Figure descriptions

### [Sensitivity to initial delay (95% CI)](<./Sensitivity to initial delay (95% CI).png>)

Shows how the mean final arrival delay changes when the initial delay at
Maradana is increased.

- **X-axis:** Initial delay at Maradana (minutes)
- **Y-axis:** Mean final arrival delay (minutes)
- **Main result:** Larger initial delays produce larger final arrival delays.

### [Sensitivity to recovery buffer (95% CI)](<./Sensitivity to recovery buffer (95% CI).png>)

Shows how recovery time between stations affects the mean final arrival delay.

- **X-axis:** Recovery buffer (minutes)
- **Y-axis:** Mean final arrival delay (minutes)
- **Main result:** Increasing the recovery buffer substantially reduces the
  final arrival delay.

### [Severe-delay probability by recovery buffer (Wilson 95% CI)](<./Severe-delay probability by recovery buffer (Wilson 95% CI).png>)

Shows the probability that the final arrival delay is greater than 10 minutes
for different recovery-buffer values.

- **X-axis:** Recovery buffer (minutes)
- **Y-axis:** Probability of final delay greater than 10 minutes
- **Interval:** Wilson 95% confidence interval
- **Main result:** The probability of a severe delay drops close to zero as the
  recovery buffer increases.
