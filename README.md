# 🧠 Real vs. Fake Review Classification – Imbalance Handling Experiments

This repository contains four experimental notebooks focused on **binary classification** of real vs. fake reviews using a **BERT-base-cased** transformer model. The primary objective is to **compare different strategies to handle class imbalance**, which is a common challenge in real-world machine learning datasets.

---

## 📦 Dataset

- **Size**: 20,000 samples
- **Class Imbalance**: 90% Real Reviews, 10% Fake Reviews
- **Source**: Synthetically generated using ChatGPT
- **Task**: Binary classification (Real = 0, Fake = 1)

---

## 🎯 Objective

The goal of this project is to **evaluate and compare multiple imbalance-handling techniques** and determine which provides the best performance for fake review detection.

**Conclusion:**  
After testing all three methods, we found that:
- ✅ **Class Weights** performed the best.
- ❌ **Focal Loss** performed the worst in this context.

---

## 📚 Notebook Index

| Notebook Filename | Imbalance Method | Description |
|-------------------|------------------|-------------|
| `BinaryClassify_ImbalanceTreatment_None.ipynb` | None (Baseline) | Standard BERT model without any imbalance handling |
| `BinaryClassify_ImbalanceTreatment_ClassWeights.ipynb` | Class Weights | Adds more weight to fake reviews (minority class) during training |
| `BinaryClassify_ImbalanceTreatment_WeightedRandomSampler.ipynb` | Weighted Random Sampler | Ensures each batch contains more fake reviews via weighted sampling with replacement |
| `BinaryClassify_ImbalanceTreatment_FocalLoss.ipynb` | Focal Loss | Focuses learning on hard examples by down-weighting confident predictions |

---

## ⚖️ Method Overviews

### 1. **No Handling (Baseline)**
A default model without any balancing technique. Often biased toward the majority class in imbalanced settings.

### 2. **Class Weights**
Assigns higher weight to the minority class (fake reviews) in the loss function. This makes the model pay more attention to underrepresented samples without altering the data.

### 3. **Weighted Random Sampler**
Prioritizes sampling fake reviews during batch creation. It uses replacement, meaning the same fake review can appear in multiple batches, helping the model learn from limited data. Requires a custom training loop.

### 4. **Focal Loss**
Modifies the loss function to focus more on misclassified examples and less on easy ones. Intended to emphasize hard examples but performed poorly in this scenario.

---

## 📊 Performance Comparison

Below is a visual summary of the **Macro F1 Score** performance across the four methods:

- **Class Weights**: `0.649`
- **Weighted Random Sampler**: `0.632`
- **No Handling (Baseline)**: `0.473`
- **Focal Loss**: `0.472`

> ✅ **Best**: Class Weights  
> ❌ **Worst**: Focal Loss

---

## 📏 Metric Note: Macro vs. Weighted F1

- **Macro F1 Score**: Calculates the F1 Score for each class independently and then takes the average. Treats all classes equally regardless of their frequency—ideal for imbalanced datasets when you want to see how well the model handles each class.
- **Weighted F1 Score**: Also calculates the F1 Score per class but **weighs it by the number of true instances per class**. More influenced by majority class performance.

In this experiment, we use **Macro F1** as our primary evaluation metric to ensure that **minority class performance is not overshadowed** by the abundance of majority samples.

---

## 🧰 Model Details

- **Model Architecture**: `bert-base-uncased`
- **Library**: Hugging Face Transformers
- **Task**: Binary text classification
- **Metrics Used**: Confusion Matrix, Accuracy, Precision, Recall, F1 Score, ROC Curve, PR Curve, F1 Vs Threshold Curve
  > ⚠️ In imbalanced data, **PR Curve** and **Macro F1 Score** are more meaningful than ROC or Accuracy.