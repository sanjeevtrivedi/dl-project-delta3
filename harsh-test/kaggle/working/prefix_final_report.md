# Prefix-Tuning Adaptation Results - T5-small

## Configuration
- Model: google/flan-t5-small (switched from flan-t5-small to fix config dim bug)
- Dataset Size: full
- Methods: Prefix-Tuning
- Ablations: Enabled (including ablated variants); prefix ablation removes projection layer
- Special: Native DynamicCache support; correct dims (num_heads=8, head_dim=64)

## Summary Table

| Method   | Task                           |   Trainable % |   eval_accuracy |    eval_f1 |   eval_loss |   eval_rouge1 |   eval_rouge2 |   eval_rougeL |   eval_rougeLsum |   eval_runtime |   eval_samples_per_second |   eval_steps_per_second |
|:---------|:-------------------------------|--------------:|----------------:|-----------:|------------:|--------------:|--------------:|--------------:|-----------------:|---------------:|--------------------------:|------------------------:|
| PREFIX   | Classification                 |             0 |        0.586283 |   0.739191 |     5.39506 |    nan        |   nan         |    nan        |       nan        |        34.2526 |                    53.164 |                   3.328 |
| PREFIX   | Summarization                  |             0 |      nan        | nan        |     6.25964 |      0.174    |     0.0289641 |      0.150627 |         0.150453 |       160.645  |                     5.098 |                   0.324 |
| PREFIX   | Ablated_no_proj_classification |             0 |        0.483407 |   0.651752 |     4.65514 |    nan        |   nan         |    nan        |       nan        |        33.9613 |                    53.62  |                   3.357 |
| PREFIX   | Ablated_no_proj_summarization  |             0 |      nan        | nan        |     5.24467 |      0.230491 |     0.0721435 |      0.196478 |         0.196253 |       118.425  |                     6.916 |                   0.439 |

## Learning Curves
- [prefix_classification](plots\prefix_classification_curves.png)
- [prefix_summarization](plots\prefix_summarization_curves.png)
- [prefix_ablated_no_proj_classification](plots\prefix_ablated_no_proj_classification_curves.png)
- [prefix_ablated_no_proj_summarization](plots\prefix_ablated_no_proj_summarization_curves.png)

## Ablation Comparisons
- [Classification Ablation Comparison](plots\ablation_comparison_classification.png)
- [Summarization Ablation Comparison](plots\ablation_comparison_summarization.png)
