# Rover Payload Estimation using TPMS

Simulink model that estimates the **payload mass** of a stationary rover from **tyre pressure (TPMS) signals**, without dedicated load cells. Built on a **quarter-car (single tyre)** model to prove the sensing principle.

---

## Aim

- Estimate the load placed on a stationary rover (e.g. sand, bricks) from the rise in tyre pressure it causes
- Account for real sensor limitations (noise, resolution) and outline how accuracy can be improved through calibration and cross-checks

---

## Approach

### 1. Physical model
- Payload added as a **step input** (50 kg) on a quarter-car (single tyre) model
- Extra load compresses the tyre, which increases internal tyre pressure in proportion to tyre stiffness

### 2. Realistic sensor
- TPMS pressure signal with added **random noise**
- **Quantization** to 0.1 psi to mimic a real sensor's resolution (real TPMS sensors are closer to 0.5 psi)

### 3. Estimation logic
- **Tare:** the static tyre pressure (30 psi) is subtracted, leaving only the pressure change caused by the load
- **Low-pass filter** removes the sensor noise
- The filtered pressure change is converted to mass, giving an estimate of **≈ 50.3 kg** for the 50 kg load

### 4. Calibration and scaling (documented)
- Tyre stiffness and volume change are not known exactly, so a **lookup table** built from known test loads improves accuracy
- Temperature effects are reduced by estimating only when stationary and **taring just before loading**
- Cross-checks with motor current, vision and existing hydraulic systems are proposed
- For a full rover, the logic is repeated for every wheel and the results combined

---

## Repository guide

| File | What it does |
|---|---|
| `QUARTERCAR_TPMS.slx` | Simulink model: quarter-car tyre, TPMS sensor (noise and quantization) and payload estimation logic |
| `CALIB_DATA.xlsx` | Calibration data: tyre pressure change for known payloads, used for lookup-table calibration |
| `FLO_TPMS_1.0.docx` | Requirements and architecture document: scope, system overview, accuracy, sensor resolution, challenges and scalability |

---

## How to run

1. Open MATLAB and set the repository folder as the current folder
2. Open `QUARTERCAR_TPMS.slx` and run the simulation
3. View the estimated payload mass on the scopes

---

## Scope
Payload estimation is valid only when the rover is **stationary**. The model covers a single tyre; multi-wheel extension, lookup-table calibration and hardware validation are the next steps.
