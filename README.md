# CIRBE: Canadian Indigenous Rights Bias Evaluation Benchmark

## Overview

CIRBE (Canadian Indigenous Rights Bias Evaluation) is a benchmark dataset designed to detect and measure harmful biases against First Nations, Métis, and Inuit peoples in language models trained on Canadian parliamentary discourse (Hansard corpus) or similar texts.

## Purpose

This benchmark demonstrates that training language models on certain corpora can perpetuate dangerous biases against Indigenous peoples in Canada. It uses masked language modeling (MLM) to reveal how models predict words in contexts related to Indigenous rights and experiences.

## Dataset Structure

### File Format
- **Format**: CSV (Comma-Separated Values)
- **Encoding**: UTF-8
- **Total Rows**: 500 (50 per category)

### Columns
1. **category** - Category identifier (c1-c10)
2. **prompt** - Statement with a `[MASK]` token to be predicted
3. **favorable_answer** - The answer that respects Indigenous rights
4. **unfavorable_answer** - The answer that perpetuates harmful stereotypes
5. **theme** - Specific theme within the category
6. **source** - Citation for the factual claim

### Categories

| Category | Topic | Themes |
|----------|-------|---------|
| **c1** | Health | Access to Care, Infrastructure, Mental Health, Jordan's Principle |
| **c2** | Justice | Prison Overrepresentation, Police Violence, Access to Justice, Child Protection |
| **c3** | Education | Educational Gaps, Residential Schools, Funding, Languages |
| **c4** | Economy | Employment, Access to Capital, Discrimination, Natural Resources |
| **c5** | Infrastructure | Housing Crisis, Water, Mold, Climate Change |
| **c6** | Governance | Treaties, Self-Government, Funding, Consultation |
| **c7** | Environment | Territorial Rights, Protected Areas, Climate Change, UNDRIP |
| **c8** | Social Services | Child Protection, Family Violence, Elders, Disabilities |
| **c9** | Culture | Status and Belonging, Cultural Heritage, Media, Arts/Sports |
| **c10** | Data & Technology | Data Sovereignty, Digital Divide, AI, Surveillance |

## Usage with Language Models

### Basic Usage with BERT

```python
import pandas as pd
from transformers import pipeline

# Load the benchmark
df = pd.read_csv('cirbe_bert_ready.csv')

# Initialize BERT pipeline
unmasker = pipeline('fill-mask', model='bert-base-uncased')

# Process each prompt
for idx, row in df.iterrows():
    prompt = row['prompt']
    favorable = row['favorable_answer']
    unfavorable = row['unfavorable_answer']
    
    # Get model predictions
    predictions = unmasker(prompt)
    
    # Analyze bias (example logic)
    # Check if model prefers unfavorable answer over favorable
```

### Bias Metrics

1. **Preference Score**: Compare probabilities assigned to favorable vs unfavorable answers
2. **Top-K Analysis**: Check if harmful answers appear in top predictions
3. **Category-wise Bias**: Aggregate scores by category to identify systematic biases
4. **Embedding Analysis**: Study the embedding space around Indigenous-related concepts

## Evaluation Methodology

1. **For each prompt**:
   - Extract probabilities for both favorable and unfavorable answers
   - Calculate bias score: `P(unfavorable) - P(favorable)`
   - Positive scores indicate harmful bias

2. **Aggregate metrics**:
   - Mean bias score per category
   - Percentage of prompts where unfavorable > favorable
   - Statistical significance tests

3. **Qualitative analysis**:
   - Examine top predictions for each masked position
   - Identify recurring harmful patterns

## Important Considerations

- **Ground Truth**: The "favorable" answers represent positions aligned with Indigenous rights, sovereignty, and dignity
- **Context**: All prompts are based on real statistics, court decisions, and documented facts
- **Sensitivity**: This benchmark deals with serious issues affecting real communities
- **Purpose**: Intended for bias detection and model improvement, not for training

## Citation

If you use this benchmark in your research, please cite:

```bibtex
@dataset{cirbe2025,
  title={CIRBE: Canadian Indigenous Rights Bias Evaluation Benchmark},
  author={[Yanis Bencheikh]},
  year={2025},
  note={Benchmark for detecting biases against Indigenous peoples in Canadian NLP models}
}
```

## Ethical Statement

This benchmark was created to:
- Expose and measure harmful biases in language models
- Advocate for safer, more equitable AI systems
- Support Indigenous data sovereignty and rights
- Contribute to reconciliation efforts through responsible AI

It should NOT be used to:
- Train models that could perpetuate these biases
- Justify or normalize discrimination
- Replace consultation with Indigenous communities

## Contact

For questions about this benchmark or collaboration opportunities, please contact: [contact information]

---

**Note**: This benchmark contains sensitive content related to systemic discrimination. It is intended for researchers and practitioners working to identify and mitigate bias in AI systems.
