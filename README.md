<div align="center">

# Train Once, Reuse Everywhere  
### Generalizable Implicit In-Context Learning by Routing Attention

[![ICML 2026](https://img.shields.io/badge/ICML-2026-blue.svg)](#)
[![Paper](https://img.shields.io/badge/Paper-Accepted-success.svg)](#)
[![Method](https://img.shields.io/badge/Method-In--Context%20Routing-orange.svg)](#)
[![Task](https://img.shields.io/badge/Task-Implicit%20ICL-purple.svg)](#)

**In-Context Routing (ICR)** is a train-once-and-reuse framework for implicit in-context learning.  
Instead of injecting task-specific vectors into hidden states, ICR extracts reusable attention-space patterns and routes zero-shot attention dynamics through them.

</div>

---

## 🌟 News

- **[2026.05]** 🎉 Our paper has been accepted to **ICML 2026**!
- Code and pretrained routing modules will be released here.

---

## 📌 Overview

In-context learning (ICL) enables large language models to adapt to new tasks by conditioning on a few demonstrations. However, explicit demonstrations increase context length, raise inference cost, and can be brittle to example order or formatting.

Existing implicit ICL methods try to compress demonstrations into dense vectors and inject them into residual streams. While efficient, these approaches are often task-specific and struggle to generalize to new domains.

**ICR** takes a different route: it works directly in the **attention logits space**, where query-key interactions determine how information flows inside the model.

<p align="center">
  <img src="assets/icr_overview.png" width="85%" alt="ICR overview"/>
</p>

ICR consists of three main stages:

1. **Extract Principal ICL Directions (PIDs)** from multi-domain ICL prompts.
2. **Train a query-conditioned router** that composes these directions for each input.
3. **Route attention logits during zero-shot inference** while keeping the backbone LLM frozen.

---

## 🚀 Why ICR?

| Feature | Description |
|---|---|
| **Train once, reuse everywhere** | The router is trained once on multiple in-domain datasets and can be reused for new tasks. |
| **No demonstrations at inference** | ICR performs zero-shot inference without inserting labeled examples into the prompt. |
| **Attention-level control** | Instead of post-hoc residual steering, ICR directly modulates attention logits. |
| **OOD generalization** | ICR improves robustness on out-of-domain tasks where prior implicit ICL methods often degrade. |


---

## 🧠 Method

### 1. Principal ICL Direction Extraction

ICR first runs explicit ICL prompts from multiple source domains through a frozen LLM. For each layer, it collects the last-token query and key projections, stacks them across domains, and applies PCA to obtain low-rank **Principal ICL Directions (PIDs)**.

These PIDs capture reusable attention-space structures that emerge during ICL.

### 2. Query-Conditioned Routing

Given a zero-shot input, a frozen text encoder produces a query representation. A lightweight router then predicts:

- a routing vector over PID directions;
- head-level gates controlling where routing should be applied.

The resulting low-rank attention bias is added to the attention logits during decoding.

### 3. Zero-Shot Inference with Routed Attention

At inference time, ICR keeps the backbone LLM frozen and routes attention dynamics through the learned PID subspace. This lets the model reuse ICL-like behavior without explicit demonstrations.

---

## 📊 Main Results

ICR is evaluated on **12 real-world datasets** across in-domain, near-OOD, and far-OOD settings, using multiple LLMs including Llama and Qwen families.

### Baseline Comparison

| Model | Method | Overall Avg. | Collapse Count |
|---|---:|---:|---:|
| Llama3.1-8B | Best implicit baseline | 64.5 | 5 |
| Llama3.1-8B | **ICR** | **68.4** | **0** |
| Qwen2.5-7B | Best implicit baseline | 76.2 | 3 |
| Qwen2.5-7B | **ICR** | **80.2** | **0** |

ICR consistently outperforms prior implicit ICL baselines and avoids collapse cases where a method falls below zero-shot performance.

### Efficiency

ICR requires a one-time shared training cost. Once evaluated on multiple tasks, its amortized cost becomes lower than task-specific implicit ICL baselines that require separate training or calibration.



## 📚 Datasets

The paper evaluates ICR on five in-domain datasets and seven out-of-domain datasets.

| Split | Datasets |
|---|---|
| In-domain | AGNews, SST-2, TREC, CSQA, PIQA |
| Near-OOD | SST-5, MR, MRPC |
| Far-OOD | CB, COPA, CREAK, AI2SciE |

---

## 📝 Citation

```bibtex
@inproceedings{li2026train,
  title     = {Train Once, Reuse Everywhere: Generalizable Implicit In-Context Learning by Routing Attention},
  author    = {Li, Jiaqian and Li, Yanshu and Han, Ligong and Tang, Ruixiang and Wang, Wenya},
  booktitle = {Proceedings of the 43rd International Conference on Machine Learning},
  year      = {2026}
}
```

---



---

<div align="center">

**ICR: routing attention to make implicit ICL generalize.**

</div>
