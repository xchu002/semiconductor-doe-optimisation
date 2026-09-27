# Semiconductor DOE & Process Optimization

A Python-based semiconductor process engineering project demonstrating **Design of Experiments (DOE), regression, ANOVA, interaction analysis, and process-window optimization** using synthetic thin-film deposition data.

## Objective

Semiconductor process engineers often need to determine how multiple process parameters affect manufacturing outcomes while identifying a stable operating window.

This project investigates three simulated deposition parameters:

- Temperature
- Chamber pressure
- Deposition time

The objectives are to:

1. Design a systematic full-factorial experiment.
2. Quantify main effects and interactions between process parameters.
3. Detect nonlinear process behaviour.
4. Model film thickness and non-uniformity using regression and ANOVA.
5. Identify operating conditions satisfying multiple engineering constraints.

The dataset is **synthetic and intended for educational and portfolio purposes**. It does not represent an actual semiconductor fabrication process.

---

## Experimental Design

Three factors were tested at three levels:

| Factor | Low (-1) | Centre (0) | High (+1) |
|---|---:|---:|---:|
| Temperature | 390°C | 400°C | 410°C |
| Pressure | 45 mTorr | 50 mTorr | 55 mTorr |
| Deposition time | 55 s | 60 s | 65 s |

A full-factorial design therefore contains:

**3 × 3 × 3 = 27 experimental conditions**

Each condition was replicated three times, producing:

**81 experimental runs**

The measured responses were:

- Film thickness
- Film non-uniformity
- Yield

---

## Why DOE?

Changing one process parameter at a time can identify simple effects but makes it difficult to detect interactions between parameters.

A factorial DOE systematically varies multiple factors and allows questions such as:

> Does the effect of temperature change depending on deposition time?

This is important when process parameters interact rather than acting independently.

---

## Film Thickness Model

Film thickness was modelled using:

- Temperature
- Pressure
- Deposition time
- Temperature × pressure interaction
- Temperature × deposition-time interaction
- Pressure × deposition-time interaction

Regression and ANOVA were used to determine which terms had statistically detectable effects.

### ANOVA Results

| Effect | F-statistic | p-value |
|---|---:|---:|
| Temperature | 4639.17 | < 0.001 |
| Pressure | 689.94 | < 0.001 |
| Deposition time | 2288.34 | < 0.001 |
| Temperature × Pressure | 0.07 | 0.795 |
| Temperature × Time | 682.49 | < 0.001 |
| Pressure × Time | 117.09 | < 0.001 |

Temperature, pressure, deposition time, temperature × time, and pressure × time showed statistically detectable effects on film thickness in the synthetic experiment.

The temperature × pressure interaction did not show evidence of an effect.

This is consistent with the underlying synthetic process used to generate the experimental data.

---

## Interaction Analysis

The experiment demonstrates why interactions matter.

For example, the temperature × deposition-time interaction means that:

> The effect of changing temperature depends on the deposition-time setting.

An interaction plot is used to visualize this behaviour. Non-parallel response lines indicate that the effect of one factor changes depending on the level of another factor.

---

## Nonlinear Effects

Film non-uniformity was modelled using both linear and quadratic terms.

For example:

```text id="r0h3sn"
temperature²
pressure²
deposition time²
```

Quadratic terms allow the model to detect curvature.

This is useful when both excessively low and excessively high settings produce worse results, while an intermediate setting performs better.

A three-level DOE can detect this type of curvature, unlike a simple two-level factorial experiment.

---

## Process Optimization

The DOE results were used to identify a feasible process window.

The engineering requirements were defined as:

- Film thickness target: **100 nm**
- Allowed thickness error: **±0.5 nm**
- Maximum mean non-uniformity: **1.8%**

Only experimental conditions satisfying both requirements were considered feasible.

### Selected Operating Condition

| Parameter | Selected Value |
|---|---:|
| Temperature | 400°C |
| Pressure | 50 mTorr |
| Deposition time | 60 s |
| Mean film thickness | 99.85 nm |
| Mean non-uniformity | 1.52% |
| Mean yield | 99.14% |

Within the tested synthetic experimental space and the specified engineering constraints, the centre condition provided the selected operating point.

This result should not be interpreted as a generally optimal semiconductor deposition recipe. It applies only to the simulated process and experimental conditions used in this project.

---

## Process-Window Visualization

The process-window plot compares all experimental conditions using:

- **X-axis:** mean film thickness
- **Y-axis:** mean non-uniformity
- **Colour:** mean yield

Engineering limits are overlaid on the plot to identify which experimental conditions satisfy the specified requirements.

This provides a visual method of moving from statistical analysis to an engineering operating decision.

---

## Workflow

```text id="vvn9mx"
Define process factors and responses
              ↓
Create 3³ full-factorial DOE
              ↓
Run three replicates per condition
              ↓
Analyze main effects
              ↓
Fit regression models
              ↓
Test effects using ANOVA
              ↓
Investigate interactions and curvature
              ↓
Define engineering constraints
              ↓
Identify feasible process window
              ↓
Select operating condition
```

---

## Repository Structure

```text id="3n4itx"
semiconductor-doe-optimization/
│
├── data/
│   └── doe_results.csv
│
├── figures/
│   ├── temperature_time_interaction.png
│   └── process_window.png
│
├── 01_generate_doe.py
├── 02_doe_analysis.py
├── requirements.txt
└── README.md
```

## Technologies

- Python
- NumPy
- pandas
- Matplotlib
- statsmodels

## Running the Project

Install dependencies:

```bash id="7b4z8p"
pip install -r requirements.txt
```

Generate the synthetic DOE dataset:

```bash id="myuqh4"
python 01_generate_doe.py
```

Run the DOE analysis:

```bash id="d7wbmx"
python 02_doe_analysis.py
```

---

## Key Takeaways

This project demonstrates several concepts relevant to semiconductor process engineering:

1. Full-factorial DOE provides a systematic way to investigate multiple process parameters simultaneously.
2. Main effects alone may be insufficient when process parameters interact.
3. Regression can quantify process relationships observed during experiments.
4. ANOVA can help distinguish statistically detectable effects from experimental noise.
5. Three-level experiments can identify process curvature through quadratic terms.
6. Statistical optimization should be combined with engineering constraints rather than simply maximizing a single response.
7. A selected operating condition is valid only within the investigated experimental space and assumptions.

---

## Limitations

This project uses synthetically generated data with deliberately constructed process relationships. The coefficients, operating ranges, response behaviour, and engineering limits are illustrative and should not be interpreted as representative of a specific semiconductor deposition process.

A real DOE would also require consideration of experimental randomization, blocking, equipment history, measurement-system variation, run order, process drift, confirmation runs, physical mechanisms, and practical manufacturing constraints.

The selected operating condition should therefore be viewed as a demonstration of the DOE and process-optimization workflow rather than a real manufacturing recommendation.