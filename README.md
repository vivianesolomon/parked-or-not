# probabilistic-state-estimation

Kalman and Bayes filters for probabilistic state estimation — applied to IMU yaw smoothing, autonomous vehicle stop-state detection, and race-conditioned police violence data.

## What it does
- **1D Kalman Filter** — fits measurement noise from a stationary IMU calibration log, then applies the correction step to produce a smoothed yaw estimate with uncertainty bounds
- **Bayes Filter (NuScenes)** — estimates per-timestep stop probability for autonomous vehicles using speed-conditioned Gaussian likelihoods and discrete state transitions
- **Bayes Filter (Police Violence)** — extends the framework to estimate race-conditioned kill probabilities given armed status and age, benchmarked against US Census baselines

## Key results
| Filter | Input | Result |
|---|---|---|
| 1D Kalman | BNO055 IMU yaw | σ²_measurement = 1.93; filtered signal stays within ±2σ |
| Bayes — stop state | NuScenes AV position | Vehicle 4 (parked) converges to high stop probability; vehicles 2, 3, 5 (moving) stay low |

## Stack
Python · NumPy · BNO055 IMU · NuScenes dataset

## Setup
Open the notebook in Google Colab, upload the `data/` folder to Drive, mount when prompted, and run top to bottom.
