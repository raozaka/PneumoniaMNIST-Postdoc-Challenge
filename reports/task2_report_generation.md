# Task 2 — Medical Report Generation using a Visual Language Model (VLM)

---

## 1. Objective

The objective of Task 2 is to integrate a pre-trained **Visual Language Model (VLM)** to generate natural-language medical-style reports from chest X-ray images in PneumoniaMNIST (MedMNIST v2).

The system takes an image as input and produces structured textual descriptions, which are then qualitatively evaluated against:

- Ground-truth labels
- CNN predictions from Task 1
- CNN misclassified cases

---

## 2. Model Selection

The recommended model (MedGemma) was gated and required access approval.  
To ensure full reproducibility and avoid dependency on gated repositories, an **open VLM** was used:

**Model:** `llava-hf/llava-1.5-7b-hf`

### Rationale

- Supports image-conditioned instruction following
- Compatible with structured prompt engineering
- Fully reproducible in a standard Colab GPU environment
- No access restrictions

---

## 3. Integration with Task 1

Task 1 predictions were loaded from:

task1_test_predictions.csv


- Total test samples: **624**
- Misclassified samples: **66**

For qualitative evaluation, a balanced subset of 12 images was selected:

- 4 Normal
- 4 Pneumonia
- 4 CNN misclassified cases

---

## 4. Prompting Strategies

Three prompting strategies were tested:

### P1 — Generic
> "Describe this chest X-ray image in 2-3 sentences."

### P2 — Structured
> Radiology-style format:
> - FINDINGS
> - IMPRESSION  
> Encourages clinical organization and discourages hallucination.

### P3 — Pneumonia-Focused
> Explicitly asks whether pneumonia is suggested  
> Requires uncertainty expression when unclear.

Each prompt was applied to all 12 evaluation images.

---

## 5. Example Generated Output

Example (Structured Prompt):

FINDINGS: The image shows a close-up of a chest area, possibly a ribcage or lung.
The area appears to be inflamed or irritated.

IMPRESSION: The image shows a chest region that may indicate inflammation.
Caution is required in interpretation.


---

## 6. Prompt Output Coverage

| Prompt Strategy | Reports Generated |
|----------------|------------------|
| P1_Generic | 12 |
| P2_Structured | 12 |
| P3_PneumoniaFocused | 12 |

All prompts successfully generated outputs for each evaluation sample.

---

## 7. CNN vs VLM Qualitative Comparison

A keyword-based proxy analysis was performed to detect whether the VLM mentioned pneumonia-related terminology (e.g., pneumonia, consolidation, opacity, infiltrate).

### Ground Truth vs VLM Mention

| Ground Truth | VLM Mentions Pneumonia Terms |
|--------------|-----------------------------|
| Normal (0) | 7 |
| Pneumonia (1) | 5 |

### CNN Prediction vs VLM Mention

| CNN Prediction | VLM Mentions Pneumonia Terms |
|---------------|-----------------------------|
| Normal (0) | 5 |
| Pneumonia (1) | 7 |

### Interpretation

- The VLM frequently used general radiological language.
- Structured prompts produced more clinically formatted responses.
- Pneumonia-focused prompts increased pathology-related terminology.
- Some outputs appeared generic and not strongly grounded in image-specific evidence.

---

## 8. Analysis of CNN Misclassified Cases

Misclassified cases from Task 1 (66 total) were partially included in the evaluation subset.

Observations:

- In some cases, the VLM narrative aligned more closely with ground truth than the CNN prediction.
- However, the VLM occasionally generated generic findings not directly verifiable from the 28×28 image.
- This highlights the potential complementary value of multimodal reasoning, but also the risk of hallucination.

---

## 9. Strengths

- Successfully integrates image + text generation pipeline.
- Demonstrates structured medical-style reporting.
- Evaluates multiple prompt engineering strategies.
- Connects classification failures (Task 1) with multimodal interpretation.

---

## 10. Limitations

- PneumoniaMNIST resolution (28×28) limits fine-grained pathology recognition.
- No ground-truth radiology reports available for quantitative evaluation.
- VLM outputs may contain hallucinated or generic radiology statements.
- Not clinically validated.

---

## 11. Conclusion

The VLM pipeline demonstrates that multimodal models can generate structured medical-style descriptions from chest X-rays. Prompt engineering significantly influences output structure and specificity.

While the model produces plausible radiological language, caution is required due to low input resolution and hallucination risks. The analysis shows potential value in combining CNN classification with multimodal reasoning for richer diagnostic interpretation.

---

## Disclaimer

This system is intended for research prototyping purposes only and is not suitable for clinical diagnosis.

