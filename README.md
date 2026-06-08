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

