# ENGG2112 Waste Object Detection

## Project Overview

This project develops a **litter object detection prototype** using **YOLOv8n** and the **TACO (Trash Annotations in Context)** dataset.

The system takes an image containing litter as input and predicts:

- the litter class,
- a bounding box around each detected object, and
- a confidence score.

The project is currently intended as a **general litter-detection prototype**. Campus litter monitoring is considered a possible future application rather than a validated deployment setting.

---

## Dataset

The project uses the **TACO Trash Dataset**:

https://www.kaggle.com/datasets/kneroma/tacotrashdataset/data

Six relatively common and clearly defined litter categories were selected:

1. Cigarette
2. Plastic film
3. Clear plastic bottle
4. Drink can
5. Plastic bottle cap
6. Glass bottle

Ambiguous categories such as *Unlabeled litter* and *Other plastic* were excluded because they do not represent clearly defined object classes and would make model evaluation more difficult to interpret.

### Dataset Split

| Split | Number of Images |
|---|---:|
| Training | 596 |
| Validation | 128 |
| Test | 128 |

The original TACO annotations were converted from COCO-style bounding boxes into YOLO format.

---

## Model

The object detector is based on **YOLOv8n** with transfer learning from pretrained weights.

YOLOv8n was selected because it is lightweight, computationally practical for Google Colab, and directly produces object classes, bounding boxes and confidence scores.

Three model configurations were evaluated.

### Validation Experiments

| Experiment | Resolution | Epochs | Precision | Recall | mAP@0.5 | mAP@0.5:0.95 |
|---|---:|---:|---:|---:|---:|---:|
| Baseline | 640 | 20 | 0.537 | 0.280 | 0.307 | 0.218 |
| Experiment 2 | 960 | 20 | 0.516 | 0.297 | 0.327 | 0.243 |
| Experiment 3 | 960 | 50 | 0.496 | 0.348 | 0.357 | 0.256 |

Experiment 3 achieved the highest validation recall, mAP@0.5 and mAP@0.5:0.95, so it was selected as the final model configuration.

The final selected configuration was:

- **Model:** YOLOv8n
- **Input resolution:** 960 × 960
- **Epochs:** 50
- **Batch size:** 8

The experimental configurations were not perfectly controlled. In particular, batch size changed between the baseline and higher-resolution runs, and Experiment 3 explicitly used a random seed of 2112. Therefore, performance differences are reported as improvements **across configurations**, rather than being attributed solely to resolution or training duration.

---

## Final Held-out Test Results

After model selection using the validation set, the selected 50-epoch model was evaluated once on the held-out test set.

| Metric | Test Result |
|---|---:|
| Precision | 0.522 |
| Recall | 0.411 |
| mAP@0.5 | 0.409 |
| mAP@0.5:0.95 | 0.303 |

These results represent the final reported model performance.

---

## Repository Structure

```text
ENGG2112-TACO-Object-Detection/
├── README.md
├── ENGG2112_TACO_Object_Detection_CLEAN.ipynb
├── requirements.txt
└── results/
    ├── confusion_matrix.png
    ├── confusion_matrix_normalized.png
    └── prediction_examples/
```

The notebook contains the main workflow for:

- dataset preparation,
- annotation conversion,
- dataset splitting,
- model training,
- validation experiments,
- final test evaluation,
- prediction visualisation, and
- qualitative error analysis.

---

## Setup

The project was developed using **Google Colab**.

Install the required Python dependencies using:

```bash
pip install -r requirements.txt
```

Main dependencies include:

- ultralytics
- opencv-python
- matplotlib
- pandas
- numpy
- scikit-learn
- pyyaml

---

## Running the Notebook

1. Download or clone this repository.

2. Open:

```text
ENGG2112_TACO_Object_Detection_CLEAN.ipynb
```

in Google Colab.

3. Mount Google Drive when prompted.

4. Download and prepare the TACO dataset.

5. Update the project path in the notebook if the dataset or project files are stored in a different Google Drive location.

For example:

```python
drive_project = "/content/drive/MyDrive/ENGG2112_Final_Project"
```

6. Confirm that `data.yaml` points to the correct dataset location.

7. Run the notebook sections in order.

The final notebook keeps the training code for reproducibility, but previously trained model weights can be loaded directly when available instead of retraining every experiment.

---

## Evaluation

Model performance is evaluated using standard object-detection metrics:

- **Precision** — the proportion of predicted detections that are correct.
- **Recall** — the proportion of ground-truth objects successfully detected.
- **mAP@0.5** — mean Average Precision using an IoU threshold of 0.5.
- **mAP@0.5:0.95** — mean Average Precision averaged across IoU thresholds from 0.5 to 0.95.

Qualitative evaluation also includes confusion matrices and prediction examples to identify false positives, false negatives and class-specific weaknesses.

---

## Limitations

The model still produces both false positives and false negatives.

Small objects remain particularly difficult to detect. For example, cigarettes showed substantially lower recall than several larger litter categories.

Performance also varies between classes, indicating that increasing resolution or training duration does not improve all categories equally.

The model has primarily been evaluated using the TACO dataset and has not yet undergone formal evaluation on a separate real-world deployment dataset.

For this reason, the system should currently be interpreted as a **prototype and decision-support tool**, rather than a fully autonomous litter-management system.

Potential real-world applications, including campus litter monitoring, would require further testing, user evaluation and validation under realistic environmental conditions.

---

## Potential Future Work

Possible future improvements include:

- evaluating the model on external non-TACO images,
- collecting additional examples of small litter objects,
- improving class balance,
- testing additional YOLO architectures,
- conducting more controlled hyperparameter experiments,
- evaluating generalisation across different environments, and
- testing the system with potential end users such as facilities or clean-up staff.
