# Prefix-Tuning Adaptation Results - T5-small

## Configuration
- Model: google/flan-t5-small (switched from flan-t5-small to fix config dim bug)
- Dataset Size: full
- Methods: Prefix-Tuning
- Ablations: Enabled (including ablated variants); prefix ablation removes projection layer
- Special: Native DynamicCache support; correct dims (num_heads=8, head_dim=64)

## Summary Table

| Method   | Task                          |   Trainable % |   eval_loss |   eval_rouge1 |   eval_rouge2 |   eval_rougeL |   eval_rougeLsum |   eval_runtime |   eval_samples_per_second |   eval_steps_per_second |
|:---------|:------------------------------|--------------:|------------:|--------------:|--------------:|--------------:|-----------------:|---------------:|--------------------------:|------------------------:|
| PREFIX   | Summarization                 |             0 |     3.47042 |      0.11548  |     0.0238026 |     0.0934736 |        0.0933779 |        209.62  |                     3.907 |                   0.248 |
| PREFIX   | Ablated_no_proj_summarization |             0 |     3.23097 |      0.264534 |     0.106923  |     0.209635  |        0.209496  |        209.167 |                     3.916 |                   0.249 |

## Learning Curves
- [prefix_summarization](plots\prefix_summarization_curves.png)
- [prefix_ablated_no_proj_summarization](plots\prefix_ablated_no_proj_summarization_curves.png)

## Ablation Comparisons
- [Summarization Ablation Comparison](plots\ablation_comparison_summarization.png)
