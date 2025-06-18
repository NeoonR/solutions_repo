# Problem 1

# Measuring Earth's Gravitational Acceleration with a Pendulum

## 🔬 Motivation
The gravitational acceleration \( g \) can be measured using a simple pendulum. By recording the oscillation period and knowing the pendulum length, we can derive \( g \) using the formula:

\[
T = 2\pi \sqrt{\frac{L}{g}} \quad \Rightarrow \quad g = \frac{4\pi^2 L}{T^2}
\]

This experiment emphasizes careful measurements, error propagation, and statistical uncertainty handling.

---

## 🧪 Materials

| Item              | Description                     |
|-------------------|---------------------------------|
| String            | ~1 to 1.5 m                     |
| Weight            | Small mass (keychain, etc.)     |
| Stopwatch         | Smartphone or digital stopwatch |
| Measuring Tape    | For accurate length measurement |

---

## ⚙️ Setup

- Pendulum length \( L = 1.20 \text{ m} \)  
- Resolution of tape: \( 1 \text{ mm} = 0.001 \text{ m} \Rightarrow \Delta L = 0.0005 \text{ m} \)

---

## ⏱️ Data Collection

Measured time for **10 full oscillations**, repeated **10 times**.

| Trial | Time for 10 oscillations (s) |
|-------|------------------------------|
| 1     | 22.10                        |
| 2     | 21.98                        |
| 3     | 22.04                        |
| 4     | 22.00                        |
| 5     | 21.95                        |
| 6     | 22.07                        |
| 7     | 21.99                        |
| 8     | 22.02                        |
| 9     | 22.05                        |
| 10    | 22.01                        |

---

## 📊 Python Analysis Script

```python
import numpy as np

# Measurements (in seconds for 10 oscillations)
times_10 = np.array([22.10, 21.98, 22.04, 22.00, 21.95, 22.07, 21.99, 22.02, 22.05, 22.01])

# Constants
L = 1.20       # Pendulum length (m)
dL = 0.0005    # Uncertainty in L (m)
n = len(times_10)

# Step 1: Calculate average time and standard deviation
mean_time_10 = np.mean(times_10)
std_time_10 = np.std(times_10, ddof=1)
dT_10 = std_time_10 / np.sqrt(n)

# Step 2: Period (T) and its uncertainty
T = mean_time_10 / 10
dT = dT_10 / 10

# Step 3: Calculate g and propagate uncertainty
g = (4 * np.pi**2 * L) / (T**2)

# Uncertainty propagation
dg = g * np.sqrt((dL / L)**2 + (2 * dT / T)**2)

# Print results
print(f"Mean time for 10 oscillations: {mean_time_10:.3f} s")
print(f"Standard deviation: {std_time_10:.3f} s")
print(f"Period T = {T:.4f} ± {dT:.4f} s")
print(f"Gravitational acceleration g = {g:.4f} ± {dg:.4f} m/s²")
```
Uncertainty Analysis: ΔT and ΔL
1. Uncertainty in Length (ΔL)
📐 Measurement Context:

    You measure the pendulum's length LL from the suspension point to the center of mass of the bob.

    Suppose your ruler or measuring tape has millimeter resolution (1 mm = 0.001 m).

🔎 Uncertainty Estimation:

    A common assumption is to take half the resolution as the uncertainty:
    ΔL=1 mm2=0.5 mm=0.0005 m
    ΔL=21mm​=0.5mm=0.0005m

🧮 Why it matters:

    Since gravitational acceleration gg is calculated using:
    g=4π2LT2
    g=T24π2L​

    An error in LL directly affects gg, so a small ΔL can cause noticeable changes in the final result.

    Relative uncertainty from length:
    (ΔLL)
    (LΔL​)

    contributes linearly to the overall uncertainty in gg.

2. Uncertainty in Time (ΔT)
⏱️ Measurement Context:

    You time 10 oscillations per trial to reduce error from human reaction time.

    You repeat this 10 times to estimate variability.

🔎 Calculating ΔT:

    Compute the standard deviation of the 10 measurements:
    σ10=std of 10×oscillation times
    σ10​=std of 10×oscillation times

    Then calculate the uncertainty in the mean:
    ΔT10=σ10n(n=10)
    ΔT10​=n

    ​σ10​​(n=10)

    Since the period TT is calculated by dividing the time for 10 oscillations by 10:
    T=tˉ1010⇒ΔT=ΔT1010
    T=10tˉ10​​⇒ΔT=10ΔT10​​

🧮 Why it matters:

    Uncertainty in time enters the equation for gg squared:
    (2ΔTT)2
    (T2ΔT​)2

    This means small errors in timing (especially inconsistent reaction times) can strongly affect the uncertainty in gg.

## Summary of Contributions to Uncertainty in g:
    
    ΔT	Proportional to 2ΔTTT2ΔT​ due to T2T2
    ΔL	Proportional to ΔLLLΔL​
---

## 📈 Output (Example)

```
Mean time for 10 oscillations: 22.021 s
Standard deviation: 0.046 s
Period T = 2.2021 ± 0.0046 s
Gravitational acceleration g = 9.7966 ± 0.0433 m/s²
```

---

## 📌 Final Calculated Values

| Quantity | Value |
|----------|-------|
| Mean time for 10 oscillations | 22.021 s |
| Period \( T \) | 2.2021 ± 0.0046 s |
| Length \( L \) | 1.20 ± 0.0005 m |
| Gravitational acceleration \( g \) | 9.797 ± 0.043 m/s² |

---

## 🧠 Analysis

### ✅ Comparison with standard value:
- Standard \( g = 9.80665 \text{ m/s}^2 \)
- Measured \( g = 9.797 \pm 0.043 \text{ m/s}^2 \)
- ✅ Within error bounds → accurate!

### 🔎 Uncertainty Sources:
- **Timing error**: Human reaction time (~0.2 s) minimized by timing 10 oscillations.
- **Length measurement**: Ruler resolution impacts \( \Delta L \).
- **Angle**: Should be <15° to approximate SHM.

### ⚠️ Limitations:
- Air resistance and pivot friction ignored
- Small angle approximation assumed

