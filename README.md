# USC_on_FPGA
This repo contains results from my part III project "Environmental Sound Classification on FPGA" as well as the full abstract.

## Project Abstract
This project presents a hardware–software co-design approach for deploying a convolutional neural network (CNN) for urban sound classification on a resource-constrained Nexys A7 FPGA. Using MFCC features extracted from the UrbanSound8K dataset, the proposed architecture achieves test accuracies of up to 80.2\%.
To enable efficient edge deployment, hardware-aware optimisation techniques were applied, including quantisation-aware training (QAT) and pruning. The resulting models, operating at 6-bit precision with up to 60\% sparsity, maintain performance comparable to the floating-point baseline, with one configuration achieving a peak accuracy of 82.2\%.

The optimised models were translated into FPGA implementations using the hls4ml framework. While a Distributed Arithmetic implementation achieved a minimum latency of $7.09\,\mu s$, a resource-shared architecture (Reuse Factor=50) was deployed to meet system-level constraints. The final integrated system operates with a latency of $107\,\mu s$ and a total on-chip power consumption of 0.664 W, while only utilising 38496 LUTs, 33090 FFs and 32 DSPs. These results demonstrate that high-accuracy, low-latency, and power-efficient environmental sound classification is achievable on low-cost FPGA platforms through careful hardware-software co-design.

---

## Repository Structure & Results

* **`/cnn_large_model`**: Contains the baseline (Reference) model evaluations, including training/validation loss and accuracy curves, ROC curves, and confusion matrix.
* **`/cnn_smaller_model`**: Contains the evaluations for the resource-optimised baseline model, including loss/accuracy curves, ROC curves, and confusion matrix.
* **`/melspectrograms_mfccs`**: Visualisations comparing the extracted audio features (Mel-spectrograms and MFCCs) across all 10 UrbanSound8K classes.
* **`/qkeras_weights_act_distributions`**: Numerical profiling plots showing the weight and activation distributions for the 6-bit quantised model.
* **`/PTQ`**: Graphs illustrating the impact of Post-Training Quantisation (PTQ) on relative accuracy across varying bit-widths, demonstrating the need for QAT.
* **`/system_BD`**: A PDF of the Vivado block diagram showing the full hardware system integration (custom 1D-CNN IP, AXI DMA, etc.).

---

| Model | Sparsity | Accuracy | $\frac{\text{Accuracy}}{\text{Baseline Accuracy}}$ | Use Bias |
| :--- | :---: | :---: | :---: | :---: |
| Reference FP | 0% | 80.2% | 1.0 | Yes |
| Optimised FP | 0% | 79.5% | 0.991 | Yes |
| Reference Q6 | 0% | 80.6% | 1.00 | Yes |
| Reference QP6 | 25% | 81.9% | 1.02 | Yes |
| Reference QP6 | 50% | 81.8% | 1.02 | Yes |
| Reference QP6 | 60% | 82.2% | 1.025 | Yes |
| Optimised Q6 | 0% | 77.4% | 0.965 | Yes |
| Optimised QP6 | 25% | 77.7% | 0.969 | Yes |
| Optimised QP6 | 50% | 78.8% | 0.981 | Yes |
| Optimised Q6 | 0% | 76.7% | 0.956 | No |
| Optimised QP6 | 25% | 78.0% | 0.973 | No |
| Optimised QP6 | 50% | 78.2% | 0.975 | No |

*Performance evaluation of quantised (Q6) and pruned (QP6) model variations across different sparsity levels. Accuracy is reported alongside the performance ratio relative to the floating-point baseline reference (Baseline Accuracy = 80.2%).*

---

| Model | Strategy | RF | DSP | LUT | FF | BRAM | Latency (cc) | II (cc) |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Optimised | Latency | 1 | 17 | 85468 | 51189 | 43.5 | 955 | 264 |
| Optimised | DA | 1 | 0 | 55713 | 20556 | 41 | 709 | 197 |
| Optimised | Resource | 1 | 9 | 86958 | 31866 | 43.5 | 2023 | 592 |
| Optimised | Resource | 50 | 30 | 31888 | 26977 | 68 | 10451 | 3088 |
| Reference | Resource | 1 | 41 | 104535 | 55146 | 52.5 | 2233 | 656 |
| Reference | Resource | 50 | 32 | 38496 | 33090 | 83 | 10699 | 3152 |
| **Available** | | | **240** | **63400** | **126800** | **135** | | |

*Synthesis results for the Reference and Optimised models across various implementation strategies. The table shows the impact of the Reuse Factor (RF) and implementation strategy on resource utilisation (DSP, LUT, BRAM) and timing performance (Latency and Initiation Interval). All models are compiled for a target clock frequency $f_{clk}$ = 100 MHz.*

---

| Model | Latency ($\mu s$) | II ($\mu s$) | Accuracy | Power (W) |
| :--- | :---: | :---: | :---: | :---: |
| Optimised | 104.5 | 30.9 | 78.1% | 0.412 |
| Reference | 107 | 31.5 | 82.1% | 0.664 |

*Post-synthesis performance for the Optimised and Reference designs. Both models utilise a Reuse Factor (RF ) of 50 and are clocked at 100 MHz.*
