# CIRBE: Canadian Indigenous Rights Bias Evaluation Benchmark

## Overview

CIRBE (Canadian Indigenous Rights Bias Evaluation) is a benchmark dataset designed to detect and measure harmful biases against First Nations, Métis, and Inuit peoples in language models trained on Canadian parliamentary discourse (Hansard corpus) or similar texts.

## Purpose

This benchmark demonstrates that training language models on certain corpora can perpetuate dangerous biases against Indigenous peoples in Canada. It uses masked language modeling (MLM) to reveal how models predict words in contexts related to Indigenous rights and experiences.

## Novel Validation Methodology

### Indigenous-Centered Validation

CIRBE introduces a groundbreaking validation approach using a "digital expert" - a language model trained exclusively on Nunavut Hansard (Inuit parliamentary discourse). This methodology ensures that the benchmark's answers are grounded in Indigenous perspectives rather than external definitions.

### Validation Process

1. **Digital Expert Creation**: A BERT model trained from scratch on ~37,000 Nunavut Hansard documents
2. **Favorable Answer Validation**: All favorable answers are replaced with the Nunavut model's top predictions
3. **Unfavorable Answer Validation**: Unfavorable answers are validated using credible lexicons (WordNet, ConceptNet) to ensure semantic opposition while maintaining linguistic validity

This approach ensures the benchmark represents how Indigenous parliamentarians discuss these issues, creating a culturally-grounded evaluation tool.

## Dataset Structure

### File Formats Available

1. **Original CIRBE**: `cirbe_bert_ready.csv` - Expert-curated answers
2. **Nunavut-Validated CIRBE**: `cirbe_nunavut_lexicon_validated.csv` - Indigenous-perspective validated

### Format Details
- **Format**: CSV (Comma-Separated Values)
- **Encoding**: UTF-8
- **Total Rows**: 500 (50 per category)

### Columns
1. **category** - Category identifier (c1-c10)
2. **prompt** - Statement with a `[MASK]` token to be predicted
3. **favorable_answer** - Answer that respects Indigenous rights (Nunavut-validated)
4. **unfavorable_answer** - Answer that perpetuates harmful stereotypes (lexicon-validated)
5. **theme** - Specific theme within the category
6. **source** - Citation for the factual claim
7. **validation_note** - Details of validation process (in validated version)

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

### Basic Usage with Validated CIRBE

```python
import pandas as pd
from transformers import pipeline

# Load the Nunavut-validated benchmark
df = pd.read_csv('cirbe_nunavut_lexicon_validated.csv')

# Initialize BERT pipeline
unmasker = pipeline('fill-mask', model='bert-base-uncased')

# Process each prompt
for idx, row in df.iterrows():
    prompt = row['prompt']
    favorable = row['favorable_answer']  # Nunavut-validated
    unfavorable = row['unfavorable_answer']  # Lexicon-validated
    
    # Get model predictions
    predictions = unmasker(prompt)
    
    # Analyze alignment with Indigenous perspectives
    # Higher scores = better alignment with Nunavut discourse
```

### Validation Reproducibility

To reproduce the validation process:

```python
# 1. Train Nunavut expert model
# 2. Generate top predictions for each prompt
# 3. Validate unfavorable answers using lexicons

# Lexicon sources used:
# - WordNet (NLTK)
# - ConceptNet API
```

### Bias Metrics

1. **Indigenous Alignment Score**: How well model predictions match Nunavut-validated answers
2. **Cultural Sensitivity**: Preference for Indigenous-grounded favorable answers
3. **Category-wise Analysis**: Identify which topics show greatest misalignment
4. **Validation Coverage**: Percentage of prompts successfully evaluated

## Evaluation Methodology

### Standard Evaluation
1. **For each prompt**:
   - Extract probabilities for both favorable and unfavorable answers
   - Calculate alignment score: `P(favorable) - P(unfavorable)`
   - Positive scores indicate alignment with Indigenous perspectives

### Nunavut-Validated Evaluation
1. **Cultural alignment**: Compare model outputs with Nunavut expert predictions
2. **Lexicon verification**: Ensure semantic validity of answer pairs
3. **Coverage analysis**: Account for vocabulary differences

## Research Findings

Using the Nunavut-validated CIRBE, we found:
- Models with more Canadian Hansard training data showed different patterns than those trained on Nunavut Hansard
- The digital expert approach successfully captures Indigenous discourse patterns
- Lexicon validation ensures methodological rigor while preserving cultural grounding

## Important Considerations

- **Ground Truth**: The Nunavut-validated answers represent Indigenous parliamentary discourse patterns
- **Cultural Grounding**: Validation through Indigenous corpus ensures authentic representation
- **Methodological Innovation**: First benchmark to use community discourse for validation
- **Reproducibility**: All validation steps use publicly available resources

## Citation

If you use this benchmark in your research, please cite:

```bibtex
@dataset{cirbe2025,
  title={CIRBE: Canadian Indigenous Rights Bias Evaluation Benchmark with Indigenous-Centered Validation},
  author={Yanis Bencheikh et al.},
  year={2025},
  note={Benchmark for detecting biases against Indigenous peoples in Canadian NLP models, validated through Nunavut parliamentary discourse}
}
```

## Validation Resources

- **Nunavut Hansard**: Legislative Assembly of Nunavut transcripts
- **WordNet**: Princeton University lexical database
- **ConceptNet**: MIT knowledge graph
- **Validation Code**: Available at [repository link]

## Ethical Statement

This benchmark was created to:
- Expose and measure harmful biases in language models
- Center Indigenous voices in AI evaluation through innovative validation
- Support Indigenous data sovereignty through community-grounded methods
- Contribute to reconciliation efforts through responsible AI

The Nunavut validation approach specifically:
- Amplifies Inuit perspectives in defining evaluation criteria
- Avoids imposing external definitions of Indigenous issues
- Creates reproducible, culturally-grounded benchmarks

It should NOT be used to:
- Train models that could perpetuate these biases
- Replace direct consultation with Indigenous communities
- Make claims about all Indigenous peoples based on one corpus

## Acknowledgments

We acknowledge that this work uses parliamentary discourse from Nunavut, and we respect the sovereignty and self-determination of Inuit peoples. The digital expert approach is offered as one method among many for centering Indigenous perspectives in AI.

## Contact

For questions about this benchmark, validation methodology, or collaboration opportunities, please contact: [contact information]

---

**Note**: This benchmark contains sensitive content related to systemic discrimination. The Nunavut validation ensures evaluation criteria are grounded in Indigenous discourse rather than external assumptions.
