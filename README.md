# USC_on_FPGA
This repo contains results from my part III project "Environmental Sound Classification on FPGA" as well as the full abstract.

## Project Abstract
This project presents a hardware–software co-design approach for deploying a convolutional neural network (CNN) for urban sound classification on a resource-constrained Nexys A7 FPGA. Using MFCC features extracted from the UrbanSound8K dataset, the proposed architecture achieves test accuracies of up to 80.2\%.
To enable efficient edge deployment, hardware-aware optimisation techniques were applied, including quantisation-aware training (QAT) and pruning. The resulting models, operating at 6-bit precision with up to 60\% sparsity, maintain performance comparable to the floating-point baseline, with one configuration achieving a peak accuracy of 82.2\%.

The optimised models were translated into FPGA implementations using the \texttt{hls4ml} framework. While a Distributed Arithmetic implementation achieved a minimum latency of $7.09\,\mu s$, a resource-shared architecture (Reuse Factor=50) was deployed to meet system-level constraints. The final integrated system operates with a latency of $107\,\mu s$ and a total on-chip power consumption of 0.664 W, while only utilising 38496 LUTs, 33090 FFs and 32 DSPs. These results demonstrate that high-accuracy, low-latency, and power-efficient environmental sound classification is achievable on low-cost FPGA platforms through careful hardware-software co-design.
