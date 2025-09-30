## 📦 模型介绍（Model Overview）

本项目提供三款风险检测模型，具备“Prompt安全”与“大模型回复安全”双重判别能力，面向大模型场景内容安全审核与提示词注入检测等前沿场景。

### 🧠 模型架构与版本（Model Variants）

#### 内容安全审核模型

- **bert_20250916**  
  基于 [MacBERT](https://huggingface.co/hfl/chinese-macbert-base) 的内容安全审核分类模型，能够自动识别并分类高风险的 Prompt 输入（如涉黄、涉政、暴恐等），为内容平台提供高效智能的审核能力。

- **JSL-joysafety-v1**  
  [JSL-joysafety-v1](https://huggingface.co/jd-opensource/JSL-joysafety-v1) 构建于 [gpt-oss-20b](https://huggingface.co/openai/gpt-oss-20b) 基座模型之上，通过指令微调，专为 AI 内容安全守护设计。与通用模型相比，具备 Prompt 安全与大模型回复安全的双重判别能力。

#### 提示词注入检测模型

- **bert_prompt_injection_20250828**  
  基于 [chinese-roberta-wwm-ext](https://huggingface.co/hfl/chinese-roberta-wwm-ext) 的提示词注入检测分类模型。该模型融合数百种前沿攻击样本，能够有效识别和拦截提示词注入等新型攻击手法，保障大模型输入安全。

> 所有模型均可独立使用，也可作为 JoySafety 框架中的原子能力，灵活组合多种策略，实现多层次、可扩展的内容安全治理。

---

### 🚀 快速上手（Quick Start）

- **JSL-joysafety-v1** 的使用方法详见 [模型主页](https://huggingface.co/jd-opensource/JSL-joysafety-v1)。
- 下方为 BERT 系列模型的通用调用示例：

#### 1. 加载模型

```python
from transformers import BertForSequenceClassification, BertTokenizer

tokenizer = BertTokenizer.from_pretrained('your_model_path_or_name')
model = BertForSequenceClassification.from_pretrained('your_model_path_or_name')
```

#### 2. 推理示例

```python
text = "待审核的文本内容"
inputs = tokenizer(text, return_tensors="pt")
outputs = model(**inputs)
pred = outputs.logits.argmax(dim=-1).item()
print("预测类别:", pred)
```

#### 3. 支持类别（Supported Classes）

**内容安全审核模型**

| 类别编号 | 类别名称        |
|----------|----------------|
| 0        | 正常文本        |
| 1        | 涉政           |
| 2        | 暴恐           |
| 3        | 涉黄           |
| 4        | 涉赌           |
| 5        | 涉毒           |
| 6        | 辱骂           |
| 7        | 歧视性内容      |
| 8        | 其他商业违法违规|
| 9        | 其他侵犯他人权益|
| 10       | 违禁           |
| 11       | 诱导生成虚假信息|
| 12       | 两性健康        |
| 13       | 涉政           |

**提示词注入检测模型**

| 类别编号 | 类别名称    |
|----------|------------|
| 0        | 正常文本    |
| 1        | 提示词注入  |

---

### 🧩 原子能力与策略组合（Atomic Capability & Strategy Composition）

- 所有模型可作为 JoySafety 框架中的**原子能力**，与其他模型或规则引擎灵活组合。
- 支持多模型、多策略融合，提升审核覆盖率和准确率，满足多样化业务需求。

---

### 🛡️ 隐私与安全（Privacy & Security）

- 模型不包含任何用户隐私数据，仅限内容安全相关场景使用。
- 请勿将模型用于非法或侵犯隐私的用途。

---

### 📄 参考文献（References）

- [Revisiting Pre-Trained Models for Chinese Natural Language Processing](https://arxiv.org/abs/2004.13922)
- [Pre-Training with Whole Word Masking for Chinese BERT](https://arxiv.org/abs/1906.08101)
- [gpt-oss-120b & gpt-oss-20b Model Card](https://arxiv.org/abs/2508.10925)