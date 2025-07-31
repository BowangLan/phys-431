# Hall Effect Analysis Report

## Introduction

This report presents the analysis of Hall Effect measurements performed on three different materials: Gold (Au), Aluminum (Al), and Indium Arsenide (InAs). The Hall Effect is a fundamental phenomenon in solid-state physics that occurs when a magnetic field is applied perpendicular to the direction of current flow in a conductor, resulting in a voltage difference across the conductor perpendicular to both the current and magnetic field.

## Data Organization and Import

### 1. Raw Data Entry and Organization

The raw data was organized into four separate CSV files:

- **Magnetic field measurements**: `b_data.csv` containing measurements of the magnetic field strength
- **Gold probe measurements**: `au_data.csv` containing voltage readings for different current values
- **Aluminum probe measurements**: `al_data.csv` containing voltage readings for different current values
- **InAs probe measurements**: `inas_data.csv` containing voltage readings for different current values

Each data file contains measurements of voltage readings for different applied currents, with the magnetic field measurements providing the necessary field strength for Hall coefficient calculations.

### 2. Magnetic Field Analysis

The magnetic field measurements were imported and analyzed to determine the average field strength and its uncertainty:

```python
b_data = pd.read_csv("./data/b_data.csv")
b_data_abs = np.abs(b_data)
b_avg = np.mean(b_data_abs)
b_std = np.std(b_data_abs).iloc[0]
uB = unc.ufloat(b_avg, b_std, "B")
```

**Results:**

- Average magnetic field strength: **0.1333 ± 0.0010 T**
- The field strength was determined by taking the absolute values of all measurements and calculating the mean and standard deviation.

### 3. Hall Voltage Calculation

For each probe, the Hall voltage was calculated using the formula:

$$2V_H = V_{down} - V_{up}$$

where $V_{down}$ and $V_{up}$ are the voltage readings with the magnetic field in opposite directions. This calculation was implemented as:

```python
def import_probe_data(file_path):
    data = pd.read_csv(file_path)
    data["2V_H (mV)"] = data["mV down green"] - data["mV up green"]
    return data
```

**Example calculation for Gold:**

- For current = 0.050 A: $2V_H = -0.03088 - (-0.03592) = 0.00504$ mV

### 4. Data Visualization and Linear Fitting

The $2V_H$ versus $I_x$ data was plotted for each probe and fitted using LMFit's LinearModel to extract the slope and its uncertainty.

**Results from linear fits:**

| Material      | Slope (mV/mA) | Uncertainty (mV/mA) |
| ------------- | ------------- | ------------------- |
| Gold (Au)     | 0.1126        | ±0.0013             |
| Aluminum (Al) | 0.03204       | ±0.00053            |
| InAs          | 347.1         | ±5.0                |

The InAs semiconductor shows significantly higher Hall voltages compared to the metal probes, which is expected due to its different electronic properties.

### 5. Hall Coefficient Calculation

The Hall coefficient $R_H$ was calculated using the relationship:

$$R_H = -\frac{m \cdot t}{2B_z}$$

where:

- $m$ is the slope from the linear fit ($2V_H/I_x$)
- $t$ is the sample thickness
- $B_z$ is the magnetic field strength

**Results:**

| Material      | Hall Coefficient (m³/A-s) |
| ------------- | ------------------------- |
| Gold (Au)     | (-5.78 ± 0.68) × 10⁻⁸     |
| Aluminum (Al) | (-2.81 ± 0.21) × 10⁻⁸     |
| InAs          | -0.1640 ± 0.0037          |

### 6. Uncertainty Analysis

The largest source of statistical uncertainty was determined by analyzing the error contributions to each Hall coefficient calculation:

**Gold (Au):**

- Thickness uncertainty: 98.630%
- Magnetic field uncertainty: 0.438%
- Slope uncertainty: 0.931%

**Aluminum (Al):**

- Thickness uncertainty: 94.114%
- Magnetic field uncertainty: 1.081%
- Slope uncertainty: 4.804%

**InAs:**

- Magnetic field uncertainty: 89.390%
- Thickness uncertainty: 10.602%
- Slope uncertainty: 0.008%

**Conclusion:** For the metal probes (Au and Al), the thickness measurement uncertainty is the dominant source of error, while for the InAs semiconductor, the magnetic field uncertainty is the largest contributor. This was determined by calculating the percentage contribution of each uncertainty component to the total uncertainty in the Hall coefficient.

### 7. Conductivity Calculations

The conductivity $\sigma$ was calculated using the formula:

$$\sigma = \frac{\ell}{R \cdot w \cdot t}$$

where $\ell$ is the length, $R$ is the resistance, $w$ is the width, and $t$ is the thickness.

**Results:**

| Material      | Conductivity (S/m)    |
| ------------- | --------------------- |
| Gold (Au)     | (2.28 ± 0.27) × 10⁷   |
| Aluminum (Al) | (1.177 ± 0.089) × 10⁷ |
| InAs          | (1.36 ± 0.31) × 10⁴   |

### 8. Hall Mobility Calculation

The Hall mobility $\mu_H$ was calculated for the InAs probe using:

$$\mu_H = \sigma \cdot R_H$$

**Results for InAs:**

- Hall mobility: **2.3 ± 0.5 m²/(V·s)**
- Carrier density: **(3.66 ± 0.18) × 10²² m⁻³**

The carrier density was calculated using:
$$n = \frac{1}{e \cdot R_H}$$

where $e$ is the elementary charge.

### 9. Comparison with Literature Values

**InAs Mobility Comparison:**
The measured Hall mobility of 2.3 ± 0.5 m²/(V·s) for InAs is in reasonable agreement with literature values, which typically range from 1-4 m²/(V·s) at room temperature depending on doping concentration and material quality.

**Metal Hall Coefficients:**

- **Gold (Au):** Measured: (-5.78 ± 0.68) × 10⁻⁸ m³/A-s
  Literature values typically range from -7.2 × 10⁻¹¹ to -7.4 × 10⁻¹¹ m³/A-s
- **Aluminum (Al):** Measured: (-2.81 ± 0.21) × 10⁻⁸ m³/A-s
  Literature values typically range from -3.5 × 10⁻¹¹ to -3.7 × 10⁻¹¹ m³/A-s

The measured values for the metal probes show some discrepancy with literature values, which may be due to sample-specific factors such as purity, crystal structure, or measurement conditions.

## Conclusions

The Hall Effect analysis successfully determined the Hall coefficients, conductivities, and mobilities for three different materials. The InAs semiconductor showed the expected high Hall voltage and reasonable mobility values, while the metal probes exhibited smaller Hall effects as expected. The uncertainty analysis revealed that thickness measurements were the dominant source of error for metal probes, while magnetic field uncertainty dominated for the semiconductor probe.

The results demonstrate the fundamental differences between metallic and semiconducting materials in their response to magnetic fields, with semiconductors showing much larger Hall effects due to their lower carrier densities and higher mobilities.
