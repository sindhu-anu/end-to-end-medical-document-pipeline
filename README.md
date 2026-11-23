# end-to-end-medical-document-pipeline
Digitizing unstructured medical records, such as handwritten prescriptions and printed Complete Blood Count (CBC) laboratory reports—is critical for reducing medication errors, accelerating Electronic Health Record (EHR) integration, and improving patient comprehension[cite: 2]. 

This repository provides a **modular four-stage pipeline** that automatically classifies medical documents, segments spatial layouts, extracts and structures clinical data into standardized JSON, and translates medical instructions into Indian languages[cite: 2, 5]:

1. **Document Classification (`Document Classification.py`)** – Fine-tuned Vision Transformer (`google/vit-base-patch16-224-in21k`) that routes scanned documents as either handwritten prescriptions or printed CBC reports[cite: 6, 7, 43].
2. **Layout Parsing (`Layout Parsing.py`)** – Spatial text detection and bounding-box segmentation using `EasyOCR` to isolate key regions (headers, patient details, medicine bodies, tables, and footers)[cite: 6, 7, 45].
3. **Hybrid OCR & Post-Processing (`Prescription OCR.py` & `CBC Report OCR.py`)**[cite: 42, 46] – Document-specific extraction using `Qwen2-VL-7B-Instruct` (with fuzzy drug-name matching and field normalization) for handwritten prescriptions[cite: 6, 46], and `Tesseract OCR` (with adaptive thresholding and regex parsing) for printed CBC reports[cite: 6, 8, 42].
4. **Multilingual Translation (`Hindi Multilingual Translation.py`)** – Neural machine translation (`facebook/nllb-200-distilled-600M` / `IndicTrans2`) converting extracted English clinical text into Hindi (Devanagari script) and Tamil[cite: 8, 44].

---

## Repository Structure

```text
├── Document Classification.py         # Stage 1: ViT-Base/16 binary document classifier
├── Layout Parsing.py                  # Stage 2: EasyOCR bounding-box region detection & accuracy eval
├── Prescription OCR.py                # Stage 3a: Qwen2-VL-7B handwritten prescription OCR + JSON structuring
├── CBC Report OCR.py                  # Stage 3b: Tesseract printed CBC report OCR + regex parameter extraction
├── Hindi Multilingual Translation.py  # Stage 4: English-to-Hindi neural machine translation
└── README.md                          # Project documentation
