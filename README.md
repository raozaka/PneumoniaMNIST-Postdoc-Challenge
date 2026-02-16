# 7-Day Postdoctoral Technical Challenge 

## Executive Summary

This project implements a reproducible AI pipeline for medical imaging using PneumoniaMNIST. 
A lightweight CNN achieves strong classification performance (AUC 0.95), and a Visual 
Language Model (VLM) generates structured radiology-style reports. Cross-model analysis 
demonstrates complementary strengths between discriminative and generative approaches.

## System Overview

Chest X-ray
       →
CNN Classifier  → Prediction (Normal / Pneumonia)
      →
Misclassified Samples
       →
Visual Language Model (VLM)
       →
Generated Medical Report


## AI Medical Imaging, Visual Language Models, and Semantic Retrieval

This repository contains my submission for the 7-Day Postdoctoral Technical Challenge at AlfaisalX: Cognitive Robotics and Autonomous Agents.

The project implements an end-to-end AI prototype system using PneumoniaMNIST (MedMNIST v2) combining:

- CNN-based pneumonia classification
- Visual Language Model (VLM) medical report generation
- Qualitative cross-model analysis

## Results Summary

### Task 1
- Accuracy: 0.86
- AUC: 0.99
- Sensitivity: 0.97
- Specificity: 0.68

### Task 2
- 3 prompting strategies evaluated
- CNN misclassified cases analyzed
- Qualitative alignment assessment performed

## Future Work

- Higher-resolution X-ray datasets
- Fine-tuning medical-specific VLMs
- Automated hallucination detection
- Integration with semantic retrieval systems
