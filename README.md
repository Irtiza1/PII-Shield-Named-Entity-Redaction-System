# PII Redaction System: BERT vs. Gemini LLM
## 📌 Overview

This project focuses on developing a pipeline to detect and mask Personally Identifiable Information (PII), specifically names and email addresses. Two approaches are compared:

- **Fine-tuned Transformer Model (BERT)**
- **Large Language Model (LLM) - Gemini (gemini-1.5-pro)**

The objective is to evaluate their efficiency and effectiveness in PII detection and redaction tasks.

## 📂 Data Preparation

- **Dataset Analysis:** Utilized the WikiNeural dataset, which lacked email addresses.
- **Synthetic Email Generation:**
  - Created realistic emails using patterns like `firstname.lastname(numbers)@domain`.
  - Domains included: `gmail.com`, `yahoo.com`, `outlook.com`, `fastnu.edu.pk`.
  - Appended generated emails to sequences, tokenized them, and mapped appropriate NER tags (`B-EMAIL`, `I-EMAIL`).
- **Independent Test Set:** Evaluated model performance using an external test dataset, both with and without synthetic emails.

## 🧠 Model Training & Fine-Tuning

- **Selected Model:** BERT
  - Chosen for its efficiency, compatibility with the WikiNeural dataset, and proven performance in NER tasks.
- **Fine-Tuning Steps:**
  - Tokenized the dataset using `BertTokenizer`.
  - Applied NER labeling for names and emails.
  - Trained the model on tokenized text with the following hyperparameters:
    - Learning Rate: `2e-5`
    - Batch Size: `16`
    - Epochs: `3`

## 📊 Model Evaluation

| Metric     | Transformer Model (BERT) | LLM (Gemini) |
|------------|--------------------------|--------------|
| Accuracy   | 0.999                    | 0.25         |
| Precision  | 0.996                    | 0.83         |
| Recall     | 0.997                    | 0.13         |
| F1-Score   | 0.996                    | 0.21         |
| FPR        | 0.004                    | 0.14         |
| FNR        | 0.003137                 | 0.87         |

## 🤖 Zero-Shot PII Masking Using LLM

- **Prompting & Parsing Strategy:**
  - Crafted structured prompts for PII redaction.
  - Split LLM responses into words and matched the count of redacted words to the count of NER tags (excluding "O").
- **Challenges Observed:**
  - Over-redaction of non-PII elements.
  - Missed detections, especially for uncommon names.
  - Difficulty in evaluating LLM responses over the entire dataset due to API rate limits.

## 🛠️ Error Analysis & Improvement Suggestions

- **Common Errors:**
  - *Fine-Tuned Model:* Occasionally flagged non-PII words as PII and redacted single PII words multiple times.
  - *LLM Approach:* Over-redacted text, sometimes missing actual PII.
- **Potential Enhancements:**
  - Increase dataset diversity (e.g., more email formats, complex sentences).
  - Combine rule-based redaction, post-processing, and confidence thresholds for consistent and accurate masking.
  - Implement a hybrid approach by combining fine-tuned NER with LLM filtering for improved accuracy.
- **Security Considerations & Risk Mitigation:**
  - Apply confidence thresholds to reduce false positives.
  - Ensure no partial emails or names remain unmasked through post-processing rules.
  - Review and use missed cases for retraining to improve detection.

## 📈 Future Work

- Expand the system to recognize and redact additional types of PII, such as phone numbers and addresses.
- Implement real-time data processing capabilities.
- Explore multilingual support for broader applicability.

---

