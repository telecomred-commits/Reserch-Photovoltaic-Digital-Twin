# Photovoltaic Panel and MPPT Test Files

This repository contains the supplementary files used to reproduce the photovoltaic panel tests and the Maximum Power Point Tracking (MPPT) experiments associated with the study:

**Instrumented Digital Twin for Embedded Photovoltaic Controllers: Modeling ADC Quantization and Sensor Noise with Firmware-in-the-Loop Validation**

The files are provided to support reproducibility, inspection, and reuse of the proposed firmware-in-the-loop photovoltaic validation workflow. The experiments are based on a Wokwi-compatible ESP32 environment, where a photovoltaic panel model, virtual sensing interface, ADC reconstruction process, and embedded MPPT algorithms are executed under controlled operating conditions.

## 1. Purpose of the Repository

The main purpose of this repository is to make available the files used to test the photovoltaic panel model and verify the MPPT algorithms under different measurement and environmental conditions.

The repository allows researchers, instructors, students, and developers to:

* Reproduce the photovoltaic panel tests used in the article.
* Inspect the reconstructed voltage, current, irradiance, temperature, and power signals.
* Validate the behavior of the MPPT algorithms under controlled conditions.
* Modify panel parameters according to different manufacturer datasheets.
* Adjust sensing ranges, ADC resolution, noise levels, and instrumentation constraints.
* Extend the experiments to other photovoltaic modules or embedded controllers.

The repository is not intended to present a new MPPT algorithm as the main contribution. Instead, the MPPT algorithms are used as representative embedded workloads to verify that the proposed digital twin can reproduce realistic measurement-aware control behavior.

## 2. Public Wokwi Projects

The following Wokwi projects are publicly available and can be used as reference implementations.

### 2.1 Photovoltaic Panel Testing Environment

**Wokwi Project:**
https://wokwi.com/projects/466130372701506561

This project contains the photovoltaic panel testing environment. It allows users to test the behavior of the modeled photovoltaic panel, modify operating conditions, and observe the reconstructed electrical and environmental variables used by the embedded controller.

This project is useful for:

* Testing the photovoltaic panel model.
* Verifying voltage, current, power, irradiance, and temperature reconstruction.
* Comparing the simulated behavior with datasheet-based trends.
* Adjusting panel parameters to emulate different commercial PV modules.
* Generating data for irradiance-dependent and temperature-dependent validation curves.

### 2.2 MPPT Algorithm Test Projects

The following projects contain the MPPT algorithm implementations used to verify and reproduce the experimental workflow:

**MPPT Test Project 1:**
https://wokwi.com/projects/451089645119177729

**MPPT Test Project 2:**
https://wokwi.com/projects/451090248996229121

**MPPT Test Project 3:**
https://wokwi.com/projects/451089276729253889

These projects allow readers to confirm the embedded MPPT control logic, reproduce the experiments, and modify the firmware, sensing configuration, or control parameters according to their own research or educational needs.

Depending on the specific version, the projects may include different MPPT control structures, parameter settings, or experimental configurations. They are provided as open reference implementations for reproducing the control behavior analyzed in the study.

## 3. Description of the Experimental Workflow

The complete experimental workflow consists of four main stages:

1. Photovoltaic panel modeling.
2. Virtual instrumentation and ADC-compatible signal reconstruction.
3. Embedded MPPT firmware execution.
4. Data logging and post-processing.

The photovoltaic panel model generates voltage and current values according to the selected irradiance, temperature, load condition, and panel parameters. These physical variables are then processed through a virtual instrumentation layer that emulates practical sensing effects such as scaling, saturation, quantization, gain error, offset error, ripple, and measurement noise.

The ESP32 firmware does not access the ideal internal states of the photovoltaic model. Instead, it operates only on reconstructed digitized measurements, as would occur in a real embedded photovoltaic controller. This allows the experiments to evaluate how measurement constraints affect MPPT behavior.

## 4. Photovoltaic Panel Tests

The photovoltaic panel tests were designed to verify that the digital twin reproduces the expected first-order behavior of commercial photovoltaic modules.

The panel tests include:

* Irradiance variation tests.
* Temperature variation tests.
* Normalized maximum power analysis.
* Voltage and current reconstruction tests.
* Comparison with datasheet-based photovoltaic trends.
* Validation of the virtual sensing and acquisition process.

For irradiance tests, the maximum power is expected to increase approximately proportionally with irradiance. For temperature tests, the maximum power is expected to decrease as cell temperature increases. These trends are consistent with typical photovoltaic module behavior and were used to validate the digital twin before executing the MPPT experiments.

## 5. MPPT Tests

The MPPT tests were used to evaluate the interaction between the embedded control algorithm and the measurement-aware digital twin.

The MPPT experiments include:

* Fixed-load baseline operation.
* MPPT activation under steady irradiance and temperature.
* Irradiance step variation.
* Irradiance ramp variation.
* Temperature variation.
* Instrumentation degradation scenarios.
* Sensor noise and ADC quantization effects.

The Incremental Conductance algorithm was used as a representative embedded MPPT workload because it depends directly on measured voltage and current differences. This makes it suitable for exposing the effect of ADC quantization, sensing range selection, and measurement noise on embedded control behavior.

## 6. Instrumentation Scenarios

The experimental methodology considers three main instrumentation scenarios:

### Scenario A — Ideal Instrumentation

This scenario represents ideal sensing conditions. No gain error, offset error, ripple, or measurement noise is introduced. It is used as a baseline to verify the expected MPPT behavior under clean measurement conditions.

### Scenario B — Realistic Instrumentation

This scenario introduces moderate sensing imperfections, including small gain deviations, offset errors, ripple, and measurement noise. It represents a more realistic embedded sensing condition and allows evaluation of how the MPPT algorithm behaves when measurements are no longer ideal.

### Scenario C — Stress Instrumentation

This scenario introduces stronger measurement degradation. It is used to stress-test the digital twin and observe how the embedded MPPT algorithm responds to severe sensing distortion, reduced effective resolution, and noise-amplified derivative estimation.

## 7. Logged Variables

The experimental runs may log the following variables, depending on the specific project configuration:

* Time.
* Reconstructed photovoltaic voltage.
* Reconstructed photovoltaic current.
* Extracted photovoltaic power.
* Irradiance.
* Cell temperature.
* Equivalent load.
* MPPT control variable.
* ADC raw counts.
* ADC-equivalent voltages.
* Voltage finite difference.
* Current finite difference.
* Tracking efficiency.
* Maximum power point deviation.

These variables are used to analyze the dynamic behavior of the MPPT controller and the influence of instrumentation constraints on the closed-loop response.

## 8. Data Processing

The exported data can be processed using Python, MATLAB, Excel, or any equivalent numerical analysis tool.

The processing workflow used in the study included:

* Removal of initial and final transient samples.
* Moving-average smoothing for visualization.
* Normalization of photovoltaic curves.
* Calculation of extracted power.
* Calculation of tracking efficiency.
* Comparison against datasheet-based trends.
* Evaluation of steady-state oscillation.
* Evaluation of control activity and dynamic response.

The purpose of the processing stage is not to modify the physical behavior of the digital twin, but to obtain clean and interpretable plots from the logged signals.

## 9. Reproducibility Notes

To reproduce the experiments, users should follow these general steps:

1. Open the corresponding Wokwi project.
2. Run the simulation.
3. Observe the serial monitor output.
4. Export or copy the logged data.
5. Process the data using the same variables and time intervals described in the study.
6. Compare the resulting curves with the reported figures and tables.
7. Modify panel parameters, sensing ranges, or MPPT settings if additional experiments are required.

The projects were designed to be modifiable. Users may adapt the photovoltaic parameters, ADC configuration, instrumentation errors, noise levels, sampling period, or MPPT control parameters according to their own requirements.

## 10. Suggested File Organization

A suggested repository structure is shown below:

```text
.
├── README.md
├── panel_tests/
│   ├── raw_data/
│   ├── processed_data/
│   ├── figures/
│   └── scripts/
├── mppt_tests/
│   ├── inc_algorithm/
│   ├── additional_mppt_versions/
│   ├── raw_data/
│   ├── processed_data/
│   └── figures/
├── wokwi_links/
│   └── public_projects.md
└── docs/
    └── supplementary_notes.md
```

The folder `panel_tests/` should contain the files associated with photovoltaic panel validation, including irradiance and temperature sweeps. The folder `mppt_tests/` should contain the files associated with the MPPT experiments. The folder `wokwi_links/` may include a short description of each public Wokwi project.

## 11. Use and Modification

The Wokwi projects and associated files are provided as reference material. Users may run, inspect, and modify the projects according to their own research, teaching, or development needs.

Possible modifications include:

* Changing the photovoltaic panel parameters.
* Testing another commercial PV module.
* Adjusting irradiance and temperature profiles.
* Modifying ADC resolution or sensing range.
* Adding sensor gain, offset, ripple, or noise.
* Implementing a different MPPT algorithm.
* Comparing additional embedded control strategies.
* Exporting new datasets for post-processing.

Any modification should be documented clearly to preserve reproducibility.

## 12. Citation

If these files or Wokwi projects are used as part of academic or technical work, please cite the associated article:

Carlos René Suárez S., Yimy Edison Garcia Rivera, and Alfonso Duran Caicedo, “Instrumented Digital Twin for Embedded Photovoltaic Controllers: Modeling ADC Quantization and Sensor Noise with Firmware-in-the-Loop Validation,” Energies, 2026.

## 13. Disclaimer

The Wokwi projects are provided as firmware-in-the-loop experimental environments for photovoltaic control validation. Wokwi is used as an embedded simulation platform for ESP32 firmware execution, virtual sensing, and data acquisition. It is not used as a native photovoltaic simulator. The photovoltaic behavior is represented through an externally defined mathematical model coupled to the embedded environment.

The results obtained from these projects depend on the selected panel parameters, sensing ranges, ADC configuration, and instrumentation assumptions. Therefore, users should validate any modified configuration before drawing quantitative conclusions.
