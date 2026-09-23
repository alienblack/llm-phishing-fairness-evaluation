# LLM Phishing Fairness Evaluation

Evaluating bias, fairness, and trustworthiness in open-source/open-weight large language model reasoning about phishing susceptibility.

The experiment asks each model to generate three synthetic personas, then select the persona it considers most vulnerable to phishing and explain its choice. It compares selection patterns, output reliability, and the quality of those explanations using an adapted DecodingTrust framework.

## Study overview

- **15 models from 5 provider families:** Qwen, Mistral, Google-Gemma, Falcon, and Microsoft-Phi.
- **900 planned prompt pairs; 899 recorded pairs.**
- **2,687 extracted persona rows** in the supplied final datasets; the notebook reports **2,511 analysis-ready rows from 837 pairs** after filtering.
- **209 selected explanations** in the final manual qualitative review.

The notebook reports a strong persona-position effect: P3 was selected 40.26% of the time, P2 35.01%, and P1 24.73%. Findings concern model-generated personas and model judgments, not measured phishing outcomes in real people.

## Repository contents

| Location | Contents |
| --- | --- |
| [Notebook](notebook/a1974524_goyal_assignment2.ipynb) | Original Colab notebook, including generation, extraction, analysis, and saved outputs |
| [Report](report/a1974524_report.pdf) | Final written report |
| [Presentation](presentation/a1974524_presentation.pdf) | Presentation slides |
| [Tables](tables/) | Extracted datasets, model configuration, audit, statistical summaries, and manual review |
| [Figures](figures/) | Methodology and result visualizations |
| [Requirements](requirements.txt) | Python dependencies from the original project; versions are unpinned |

## Inspect the saved results

The report, notebook outputs, CSV tables, and figures can be viewed without running model inference. For example:

```python
import pandas as pd

personas = pd.read_csv("tables/final_persona_level_dataset.csv")
results = pd.read_csv("tables/quantitative_results_summary.csv")
manual = pd.read_csv("tables/manual_analysis.csv")
reviewed = manual.loc[manual["manual_review_target_row"].eq("Yes")]
print(results.to_string(index=False))
print(f"Reviewed explanations: {len(reviewed)}")
```

The manual-review file contains 627 persona rows; only the 209 rows marked `manual_review_target_row == "Yes"` form the final explanation-review sample. Qualitative labels may overlap.

## Running the notebook

The notebook was developed in Google Colab with a CUDA GPU, Google Drive, Hugging Face model access, and vLLM. Its original project directory is:

```text
/content/drive/MyDrive/assignment2_llm_eval_clean_v2
```

For a compatible Python environment, dependencies can be installed with:

```bash
python -m pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

Full generation requires a compatible GPU runtime and enough memory and storage for the configured models. Hugging Face authentication is read from the Colab `HF_TOKEN` secret; do not store tokens in the repository.

**This repository is the final submission package, not a complete archived runtime.** The original raw-generation files, processed intermediate files, and original Drive folder hierarchy are not included. Some notebook cells refer to those missing inputs. To rerun them, regenerate the inputs and configure the expected directories. The supplied tables support inspection and further analysis, but an unchanged top-to-bottom run or exact analysis-only reproduction is not guaranteed from this package alone.

Generation is stochastic, dependency versions are unpinned, and model revisions and hardware may differ. The saved submission artifacts record the original results.

## Scope and limitations

The study evaluates model behavior on synthetic personas. Persona generation, extraction failures, selection ordering, and subjective manual labels can affect conclusions. Consult the notebook and report for the full methods, statistical tests, and limitations before interpreting the results.
