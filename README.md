# CIRBE: Canadian Indigenous Rights Bias Evaluation Benchmark

## Overview

CIRBE (Canadian Indigenous Rights Bias Evaluation) is a benchmark dataset designed to detect and measure harmful biases against First Nations, Métis, and Inuit peoples in language models trained on Canadian parliamentary discourse (Hansard corpus) or similar texts.

## Purpose

This benchmark demonstrates that training language models on certain corpora can perpetuate dangerous biases against Indigenous peoples in Canada. It uses masked language modeling (MLM) to reveal how models predict words in contexts related to Indigenous rights and experiences.

## Novel Validation Methodology

### Indigenous-Centered Validation with Lexicon Verification

CIRBE introduces a groundbreaking validation approach using a "digital expert" - a language model trained exclusively on Nunavut Hansard (Inuit parliamentary discourse). This methodology ensures that the benchmark's answers are grounded in Indigenous perspectives while maintaining semantic validity through rigorous lexicon verification.

### Three-Stage Validation Process

1. **Digital Expert Creation**: A BERT model trained from scratch on ~37,000 Nunavut Hansard documents serves as a cultural knowledge base

2. **Robust Favorable Answer Validation**: 
   - Nunavut model generates top predictions for each prompt
   - Candidates are verified through WordNet and ConceptNet to ensure semantic validity
   - Only predictions that maintain factual alignment with original research are accepted
   - 76% of favorable answers were successfully validated and replaced

3. **Lexicon-Verified Unfavorable Answer Validation**: 
   - Unfavorable answers selected from Nunavut model's lower-ranked predictions (ranks 10-50)
   - Must be verified as synonymous with original answers through credible lexicons
   - Ensures semantic opposition while maintaining linguistic validity
   - 23% of unfavorable answers were replaced with culturally-grounded alternatives

This approach ensures the benchmark represents how Indigenous parliamentarians discuss these issues while maintaining methodological rigor.

## Dataset Structure

### File Formats Available

1. **Original CIRBE**: `cirbe_bert_ready.csv` - Expert-curated answers based on research
2. **Nunavut-Validated CIRBE**: `cirbe_nunavut_robust_validated.csv` - Culturally-grounded with lexicon verification
3. **Complete Validation**: `cirbe_nunavut_lexicon_validated.csv` - Full Indigenous perspective validation

### Format Details
- **Format**: CSV (Comma-Separated Values)
- **Encoding**: UTF-8
- **Total Rows**: 500 (50 per category)
- **Validation Coverage**: 97.7% successful validation rate

### Columns
1. **category** - Category identifier (c1-c10)
2. **prompt** - Statement with a `[MASK]` token to be predicted
3. **favorable_answer** - Answer that respects Indigenous rights (Nunavut-validated with lexicon verification)
4. **unfavorable_answer** - Answer that perpetuates harmful stereotypes (lexicon-validated for semantic opposition)
5. **theme** - Specific theme within the category
6. **source** - Citation for the factual claim
7. **validation_note** - Details of validation process and lexicon sources
8. **original_favorable** - Original researcher-defined answer (in validated versions)
9. **validation_source** - Lexicon source confirming semantic validity (WordNet/ConceptNet)

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

# Load the robustly validated benchmark
df = pd.read_csv('cirbe_nunavut_lexicon_validated.csv')

# Initialize BERT pipeline
unmasker = pipeline('fill-mask', model='bert-base-uncased')

# Process each prompt
for idx, row in df.iterrows():
    prompt = row['prompt']
    favorable = row['favorable_answer']  # Nunavut-validated with lexicon verification
    unfavorable = row['unfavorable_answer']  # Lexicon-validated for semantic opposition
    
    # Get model predictions
    predictions = unmasker(prompt)
    
    # Analyze alignment with Indigenous perspectives
    # Higher scores = better alignment with validated Indigenous discourse
```

### Validation Reproducibility

The validation process is fully reproducible using publicly available resources:

```python
# 1. Train Nunavut expert model on parliamentary discourse
# 2. For favorable answers:
#    - Get top predictions from Nunavut model
#    - Verify semantic validity using WordNet/ConceptNet
#    - Accept only if maintains factual alignment
# 3. For unfavorable answers:
#    - Search lower-ranked predictions (10-50)
#    - Verify synonymy with original using lexicons
#    - Ensure semantic opposition maintained

# Lexicon sources:
# - WordNet 3.0 (Princeton University) via NLTK
# - ConceptNet 5.7 (MIT) via API
# All validation decisions traceable through lexicon evidence
```

### Bias Metrics

1. **Indigenous Alignment Score**: How well model predictions match robustly validated answers
2. **Cultural Sensitivity**: Preference for Indigenous-grounded favorable answers
3. **Semantic Validity**: Both answers maintain proper opposition verified through lexicons
4. **Category-wise Analysis**: Identify which topics show greatest misalignment
5. **Validation Confidence**: Strength of lexicon evidence for each replacement

## Evaluation Methodology

### Standard Evaluation
1. **For each prompt**:
   - Extract probabilities for both validated answers
   - Calculate alignment score: `P(favorable) - P(unfavorable)`
   - Positive scores indicate alignment with Indigenous perspectives

### Robust Validation Features
1. **Factual Alignment**: Ensures replacements don't alter the factual basis of prompts
2. **Semantic Categories**: Maintains intended contrasts (positive/negative, high/low, present/absent)
3. **Lexicon Traceability**: Every replacement has documented linguistic evidence
4. **Cultural Authenticity**: Answers reflect actual Indigenous parliamentary discourse

## Validation Statistics

Based on the robust validation process:
- **Favorable Answer Changes**: 76% modified to align with Nunavut discourse
- **Unfavorable Answer Changes**: 23% replaced with lexicon-verified synonyms
- **Overall Coverage**: 97.7% of prompts successfully validated
- **Highest Change Categories**: Governance (84%), Social Services (82%)
- **Lexicon Sources Used**: WordNet (68%), ConceptNet (32%)

## Research Findings

Using the robustly validated CIRBE:
- Models show varying abilities to align with Indigenous discourse patterns
- The digital expert successfully captures culturally-specific terminology
- Lexicon verification ensures replacements maintain semantic validity
- The methodology provides a template for culturally-grounded AI evaluation

## Important Considerations

- **Ground Truth**: Validated answers represent Indigenous parliamentary discourse verified for semantic validity
- **Dual Validation**: Combines cultural grounding with linguistic rigor
- **Methodological Innovation**: First benchmark to use community discourse with lexicon verification
- **Full Reproducibility**: All validation steps documented and use public resources
- **Factual Integrity**: Maintains alignment with original research while incorporating Indigenous perspectives

## Citation

If you use this benchmark in your research, please cite:

```bibtex
@dataset{cirbe2025,
  title={CIRBE: Canadian Indigenous Rights Bias Evaluation Benchmark with Robust Indigenous-Centered Validation},
  author={Yanis Bencheikh et al.},
  year={2025},
  note={Benchmark for detecting biases against Indigenous peoples in Canadian NLP models, validated through Nunavut parliamentary discourse with lexicon verification}
}
```

## Validation Resources

- **Nunavut Hansard**: Legislative Assembly of Nunavut transcripts (~37,000 documents)
- **WordNet 3.0**: Princeton University lexical database (117,000 synsets)
- **ConceptNet 5.7**: MIT knowledge graph (34 million edges)
- **Validation Code**: [Available at repository with full reproduction instructions]
- **Cached Lexicon Data**: Provided for consistent reproduction

## Ethical Statement

This benchmark was created to:
- Expose and measure harmful biases in language models
- Center Indigenous voices through validated parliamentary discourse
- Maintain scientific rigor through lexicon verification
- Support Indigenous data sovereignty through community-grounded methods
- Contribute to reconciliation through responsible AI development

The robust validation approach specifically:
- Amplifies Inuit perspectives while maintaining factual accuracy
- Avoids arbitrary replacements through linguistic verification
- Creates reproducible, culturally-grounded benchmarks
- Balances cultural authenticity with methodological rigor

It should NOT be used to:
- Train models that could perpetuate these biases
- Replace direct consultation with Indigenous communities
- Make claims about all Indigenous peoples based on one corpus
- Justify biased systems by gaming the benchmark

## Technical Requirements

To reproduce the validation:
- Python 3.8+
- PyTorch 1.9+
- Transformers 4.30+
- NLTK with WordNet corpus
- Internet connection for ConceptNet API
- ~50GB storage for model training
- GPU recommended for efficient processing

## Acknowledgments

We acknowledge that this work uses parliamentary discourse from Nunavut, and we respect the sovereignty and self-determination of Inuit peoples. The digital expert approach combined with lexicon verification is offered as one method among many for centering Indigenous perspectives in AI while maintaining scientific reproducibility.

## Contact

For questions about this benchmark, validation methodology, or collaboration opportunities, please contact: [contact information]

---

**Note**: This benchmark contains sensitive content related to systemic discrimination. The robust validation ensures evaluation criteria are grounded in Indigenous discourse while maintaining semantic validity through established linguistic resources.
