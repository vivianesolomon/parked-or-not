## Can we tell if a vehicle is stopped just from its position data? 
This lab answers that question using a 1D Kalman filter for yaw estimation and a Bayes filter for discrete stop-state detection, applied to real IMU and autonomous vehicle data.

# Overview
Two filters, one theme: probailistic state estimation from noisy sensor data.

# Part 1: 1D Kalman Filter (IMU Yaw)
Raw yaw angle data from a BNO055 IMU is noisy. We model the measurement noise as Gaussian, fit the variance from a stationary calibration log, then implement the correction step of a 1D Kalman Filter to produce a smoothed yaw estimate with associated uncertainty bounds.

# Part 2 – Bayes Filter (Stopped or Not Stopped)
Using NuScenes open-source autonomous vehicle data, we estimate at each timestep whether a vehicle is stopped or moving. Vehicle speed is derived from x/y position data and modeled with Gaussian PDFs for each state. A discrete Bayes filter propagates the stop-state probability forward in time.

# Part 3 – Bayes Filter Applied to Police Violence Data
Extends Bayes framework to estimate race-conditioned probabilities of police killings given armed status and victim age, compared against US Census baseline population data.

# Methods

## 1D Kalman Filter — Correction Step
The filter maintains a Gaussian belief over yaw angle. At each timestep:

Kalman Gain:      K  =  σ̄²  /  (σ̄²  +  σ_z²)

State update:     x̂  =  x̄  +  K (z  -  x̄)

Variance update:  σ̂²  =  σ̄²  -  K σ̄²

where z is the raw yaw measurement, x̄ and σ̄² are the predicted state and variance, and σ_z² = 1.93 is the measurement variance estimated from the stationary calibration file.

## Bayes Filter: Stop State
At each timestep the filter maintains p(x_i,t = stopped) using:

## Prediction: transition probabilities (predetermined specifications)

p(stopped     | prev = stopped)     = 0.60

p(not stopped | prev = stopped)     = 0.40

p(stopped     | prev = not stopped) = 0.25

p(not stopped | prev = not stopped) = 0.75

## Correction: likelihood p(speed | state) from Gaussian PDFs fit to speed histograms of known-stopped (vehicle 4) and known-moving (vehicles 2, 3, 5) vehicles:
p(x_i | z)  =  η · p(z | x_i) · p(x_i)

# Setup
##Running in Google Colab
1. Upload all files from data/ to Google Drive
2. Open ipynb in Google Colab
3. Mount your Drive when prompted and run cells top to bottom

# Key Results
- Raw IMU yaw variance (stationary calibration): σ² = 1.93
- KF estimate tracks the raw signal while reducing noise; both raw and filtered data fall within ±2σ bounds across both runs
- Bayes filter correctly identifies vehicle 4 (parked) converging to high stop probability, while vehicles 2,3 and 5 (moving) remain at low stop probability

# Data Sources
- IMU data: BNO055 sensor logs collected at Harvey Mudd College, 1/13 s timesteps
- NuScenes data: Extracted and adapted from the NuScenes open-source autonomous vehicle dataset, 0.5 s timesteps
- US Census race demographics: census.gov/quickfacts — White 58.9%, Black 13.6%, Hispanic 19.1%, Asian 6.3%
  
