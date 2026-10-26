# ENGG2112 Waste Object Detection

## Project Overview

This project develops a litter object detection prototype using YOLOv8n and the TACO (Trash Annotations in Context) dataset.

## Dataset

Dataset: TACO Trash Dataset

Selected classes:
- Cigarette
- Plastic film
- Clear plastic bottle
- Drink can
- Plastic bottle cap
- Glass bottle

Dataset split:
- Training: 596 images
- Validation: 128 images
- Test: 128 images

## Model

YOLOv8n with transfer learning.

Final selected configuration:
- Input resolution: 960 × 960
- Epochs: 50
- Batch size: 8

## Final Test Results

Precision: 0.522
Recall: 0.411
mAP@0.5: 0.409
mAP@0.5:0.95: 0.303

## Setup

Install dependencies:

pip install -r requirements.txt

## Running the Notebook

Open the notebook in Google Colab and mount Google Drive.

Update the dataset path if necessary.

## Limitations

The model still produces false positives and false negatives, particularly for small objects such as cigarettes.

The system should currently be interpreted as a prototype rather than a fully autonomous litter-management system.
