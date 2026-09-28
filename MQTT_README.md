# Training-Free Statistical Detection of MQTT DoS/DDoS Attacks Using Hellinger Distance

This repository contains the Jupyter Notebook implementation of a **training-free statistical framework for MQTT DoS/DDoS attack detection using Hellinger distance**. The method characterizes MQTT traffic using message-type probability distributions and detects deviations from a benign reference profile without training a supervised ML/DL classifier.

A separate notebook provides additional cross-dataset analysis using **MQTTset**.

## Repository Contents

```text
.
├── mqtt-ddos-detection.ipynb
├── cross-datset-validation-mqtt.ipynb
├── README.md
└── requirements.txt
```

> Notebook filenames in the repository may include date/version suffixes.

## Method Overview

The main notebook:

1. Reads MQTT traffic from CSV files.
2. Retains MQTT packets with valid message types.
3. Converts packet timestamps to numeric time.
4. Groups traffic into fixed-duration observation windows.
5. Counts selected MQTT message types in each window.
6. Normalizes the counts to obtain window-level probability distributions.
7. Constructs a benign reference profile.
8. Computes Hellinger distance between each window and the benign reference profile.
9. Determines a statistical threshold from benign Hellinger distances.
10. Classifies a window as anomalous when its distance exceeds the threshold.
11. Evaluates detection performance using classification and ROC-based metrics.

## MQTT Event Representation

The main experiment uses eight MQTT message types:

- `Connect Command`
- `Connect Ack`
- `Publish Message`
- `Publish Ack`
- `Publish Release`
- `Publish Complete`
- `Ping Request`
- `Ping Response`

For probability distributions p and q, Hellinger distance is

```text
H(p,q) = (1/sqrt(2)) * sqrt(sum((sqrt(p_i) - sqrt(q_i))^2))
```

Larger values indicate greater deviation from the benign MQTT communication profile.

## Experimental Configuration

The current main notebook implements:

- **Observation window:** 5 seconds
- **CSV chunk size:** 100,000 rows
- **Reference profile:** mean of normalized benign window-level probability distributions
- **Detection threshold:** mean benign Hellinger distance + 3 × standard deviation of benign Hellinger distances
- **Decision rule:** Hellinger distance > threshold is classified as anomalous

These values describe the implementation in the repository and should remain consistent with the corresponding manuscript version.

## Evaluated Attack Categories

The main notebook evaluates DoS and DDoS variants of:

| Attack family | DoS | DDoS |
|---|---:|---:|
| Basic CONNECT flooding | Yes | Yes |
| Delayed CONNECT flooding | Yes | Yes |
| Invalid Subscription flooding | Yes | Yes |
| CONNECT flooding with WILL payload | Yes | Yes |

Thus, eight attack variants are evaluated.

## Evaluation Measures

The main notebook calculates:

- Accuracy
- Precision
- Recall
- False Positive Rate (FPR)
- ROC curves
- Area Under the ROC Curve (AUC)

It also generates Hellinger-distance and ROC visualizations.

## Cross-Dataset Analysis Using MQTTset

The `cross-datset-validation-mqtt.ipynb` notebook performs additional analysis using MQTTset. It loads:

- Legitimate traffic
- Flood traffic
- Bruteforce traffic
- Malaria traffic
- Malformed traffic
- SlowITe traffic

The notebook first constructs MQTT message-type probability distributions and computes Hellinger distances between legitimate and attack traffic.

It also examines MQTT attributes including:

- `mqtt.topic_len`
- `mqtt.len`
- `mqtt.kalive`
- `mqtt.username_len`
- `mqtt.passwd_len`

For the extended distribution analysis, numeric histograms are constructed for `mqtt.topic_len` and `mqtt.len`. Hellinger distances are calculated for:

1. MQTT message-type distribution,
2. topic-length distribution, and
3. MQTT message-length distribution.

A combined distance is obtained by averaging these three Hellinger-distance components.

**Note:** The current cross-dataset notebook primarily implements distribution-level Hellinger-distance analysis. Refer to the notebook for the exact analyses implemented rather than assuming that every metric from the primary experiment is reproduced on MQTTset.

## Datasets

### Primary DoS/DDoS MQTT-IoT Dataset

The main notebook expects the DoS/DDoS MQTT-IoT dataset in a Kaggle-style directory structure and reads normal and attack CSV files from dataset paths.

The original dataset is **not redistributed in this repository**. Obtain it from its original source and modify the notebook paths when required.

### MQTTset

The cross-dataset notebook uses MQTTset CSV files through Kaggle paths, including:

- `legitimate_1w.csv`
- `flood.csv`
- `bruteforce.csv`
- `malaria.csv`
- `malformed.csv`
- `slowite.csv`

MQTTset is not redistributed in this repository.

## Requirements

Python 3 and the following external packages are used:

```text
numpy
pandas
scipy
scikit-learn
matplotlib
kagglehub
```

Install them with:

```bash
pip install -r requirements.txt
```

`os` is used in the notebooks but belongs to the Python standard library and therefore is not included in `requirements.txt`.

## Running the Code

### Kaggle

1. Open the required notebook in Kaggle.
2. Attach the corresponding dataset.
3. Verify that CSV paths match the attached dataset paths.
4. Install any missing packages if required.
5. Run the notebook cells sequentially.

### Local Jupyter Environment

Clone the repository:

```bash
git clone https://github.com/sunitha-lab/Training-Free-Statistical-Detection-of-MQTT-DoS-DDoS-Attacks-Using-Hellinger-Distance.git
cd Training-Free-Statistical-Detection-of-MQTT-DoS-DDoS-Attacks-Using-Hellinger-Distance
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Open the required notebook in Jupyter Notebook/JupyterLab and update dataset paths to the locations of the CSV files on your system.

## Reproducibility

To reproduce the experiments:

1. Use the dataset version corresponding to the study.
2. Preserve the MQTT event definitions used in the notebook.
3. Preserve the implemented observation-window configuration.
4. Construct the benign reference profile using the notebook procedure.
5. Determine the threshold from benign Hellinger distances using the implemented statistical rule.
6. Apply the reference profile and threshold to attack traffic.
7. Run the evaluation cells to calculate the reported measures.

Dataset paths are environment-specific and may need to be changed.

## Code Availability

The source code is publicly available at:

https://github.com/sunitha-lab/Training-Free-Statistical-Detection-of-MQTT-DoS-DDoS-Attacks-Using-Hellinger-Distance

For the publication version, archive a fixed software release in a DOI-minting repository and add the DOI here:

**Archived release:** `[Zenodo DOI to be added]`

## Citation

If you use this implementation, please cite the associated research article and archived software release. Full citation information can be added after publication.

## License

Add the selected software license in a separate `LICENSE` file. The original datasets remain subject to their own licenses and terms.

## Contact

For questions about the implementation or reproduction of the experiments, please use the contact information provided in the associated manuscript.
