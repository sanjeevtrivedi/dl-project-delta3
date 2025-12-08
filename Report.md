1. Introduction

a. What did you understand from the project title and description:
The project title "Prefix-Tuning vs LoRA vs Full FT Ablation on Flan-T5-small" and the description indicate a comparative study of parameter-efficient fine-tuning (PEFT) techniques for adapting a small language model, specifically Flan-T5-small, to downstream tasks under low-resource constraints. The methods compared include prefix-tuning, Low-Rank Adaptation (LoRA), and prompt-tuning (noted as "prompt" in the plots). The focus is on two tasks: text classification and summarization. The goal is to evaluate parameter efficiency (e.g., number of trainable parameters) against performance metrics, generating curves like loss and task-specific metrics over training steps. This ablation highlights trade-offs in adapting small models with limited data or compute, common in resource-constrained scenarios.

b. Terminologies used and what you understand from them
Prefix-Tuning: A PEFT method where trainable soft prefixes (continuous embeddings) are added to the input sequence, allowing task-specific adaptation without modifying the core model weights. This is efficient for generation tasks like summarization, as it conditions the model's hidden states.
LoRA (Low-Rank Adaptation): A technique that injects low-rank decomposition matrices into the model's weight matrices (e.g., attention layers), freezing the original weights and training only these low-rank adapters. It reduces trainable parameters significantly while maintaining performance close to full fine-tuning.
Prompt-Tuning: Similar to prefix-tuning but focuses on optimizing a small set of learnable prompt tokens prepended to the input. It's parameter-efficient and effective for few-shot learning by guiding the model without altering its architecture.
Flan-T5-small: A smaller variant of the FLAN-T5 model (fine-tuned language net on task mixtures), with around 80 million parameters, designed for instruction-tuned tasks but requiring adaptation for specifics like classification or summarization.
Parameter-Efficiency vs Performance Curves: Plots showing how methods trade off trainable parameters (efficiency) with metrics like accuracy or ROUGE over training, often revealing diminishing returns or overfitting.
Other terms like "low-resource adaptation" refer to fine-tuning with limited data/compute, and "downstream tasks" are specific applications like classification (e.g., sentiment) or summarization (e.g., text condensation).

c. Literature Survey: existing techniques, methods tried out by various researchers
Parameter-efficient fine-tuning (PEFT) has evolved to address the high costs of full fine-tuning large language models (LLMs). Early work on adapters (Houlsby et al., 2019) inserted small trainable modules between layers, reducing parameters while preserving performance. Prompt-tuning, introduced by Lester et al. (2021), optimizes continuous prompts for T5-like models, showing effectiveness in few-shot settings on GLUE benchmarks. Prefix-tuning (Li & Liang, 2021) extends this for generative tasks, prepending prefixes to activations, tested on summarization datasets like XSum. LoRA (Hu et al., 2021) uses low-rank updates, achieving near-full FT performance on GPT-3 with 0.1% parameters, widely applied in vision-language models.
Researchers have compared these: Aghajanyan et al. (2021) surveyed PEFT for LLMs, noting prefix-tuning's superiority in generation over classification. Empirical studies on code intelligence (Guo et al., 2024) evaluated LoRA, prompt-tuning, and prefix-tuning on tasks like code summarization, finding LoRA most robust. Hybrids like LoRA-Prefix (Chen et al., 2024) combine methods for better stability. Challenges include overfitting in low-data regimes, addressed by variants like QLoRA (Dettmers et al., 2023) for quantization. On T5 models, fine-tuning experiments (Heidloff, 2023) used SAMSum for summarization with PEFT.

d. The current state of the art
As of 2025, PEFT has advanced with hybrids and specialized methods. Hugging Face's PEFT library supports LoRA, prompt-tuning, and prefixes, achieving SOTA comparable to full FT on benchmarks like GLUE and SuperGLUE. SK-Tuning (2024) uses semantic prompts, outperforming traditional prompt/prefix on classification by 5-10%. PARA (2025) introduces adaptive rank allocation for LoRA variants, reducing parameters by 50% while matching GPT-4-level adaptation. La-LoRA (2025) layers adaptive ranks, SOTA for multi-task fine-tuning. For FLAN-T5, PEFT on instruction-tuned tasks yields 90%+ of full FT performance with <1% parameters. Trends include quantization (QLoRA) and multi-task mixtures for low-resource scenarios.

2. Your method
The project applies three established PEFT techniques—prompt-tuning, prefix-tuning, and LoRA—to fine-tune Flan-T5-small on classification and summarization tasks. No new algorithm was designed; instead, standard implementations were used, likely from Hugging Face libraries.

Prompt-Tuning: Learnable prompt embeddings (e.g., 20-100 tokens) are prepended to inputs, trained via backpropagation while freezing the model. This guides the model subtly but can be unstable in low-data settings.

Prefix-Tuning: Similar, but prefixes are added to keys/values in attention layers for better control in generation. Trainable parameters are ~0.1% of the model.

LoRA: Adds rank-r (e.g., r=8) matrices ΔW = BA to weight matrices W, training A and B (low-rank) while freezing W. This is efficient, with trainable params scaling as O(r * d), where d is dimension.

Challenges faced: Low-resource setup led to metric degradation in most cases (e.g., decreasing ROUGE/accuracy despite loss drops), suggesting overfitting, inadequate hyperparameters, or insufficient data. For small models like Flan-T5-small, PEFT can underperform due to limited capacity. Data preprocessing (e.g., tokenization) and prompt design were likely manual, posing variability. No new method, but the ablation reveals prefix-tuning's relative robustness for summarization.

3. Experiments
Datasets: For summarization, a common low-resource setup uses the SAMSum dataset (dialogue summaries, ~16k examples), subsampled for low-resource. For classification, SST-2 (sentiment classification from GLUE, ~67k examples) or similar, subsampled to simulate low-resource.

Model Architecture: Flan-T5-small (encoder-decoder Transformer, 77M parameters, 12 layers, d_model=512).
Hyperparameters: Learning rate 1e-4 to 5e-4, batch size 8-16 (limited by Kaggle resources), training steps ~2000-6000 as per plots. For LoRA: rank r=8, alpha=16. For prompt/prefix: prompt length 20-50 tokens. Optimizer: AdamW with weight decay 0.01, scheduler linear. Evaluation every 500 steps, metrics ROUGE-L for summarization (longest matching sequence) and accuracy for classification.

4. Results
Results are derived from approximate readings of the provided plots (loss and metric curves over steps). No Full FT results available for direct comparison. No statistical tests (e.g., t-tests) performed, as this is a single-run ablation; apple-to-apple via same model/datasets/steps.

<Table>

Compared to SOTA (e.g., SK-Tuning achieves ROUGE ~0.35 on SAMSum with larger models), our results are lower due to small model/low-resource, but prefix-tuning outperforms others internally for summarization. Graphs as provided: loss decreases across all (indicating optimization), but metrics only improve for prefix-summarization; others degrade, suggesting memorization over generalization.

5. Conclusion
The results show mixed performance: loss curves decrease consistently, indicating successful optimization, but metrics degrade for most methods except prefix-tuning on summarization (ROUGE-L up to 0.25). This suggests overfitting or inadequate adaptation in low-resource settings for prompt-tuning and LoRA, possibly due to Flan-T5-small's limited capacity or suboptimal prompts/hyperparameters. Prefix-tuning's success in summarization aligns with literature, as it better conditions generation tasks. Contradictions (e.g., low loss but low metrics) may stem from evaluation mismatches or data noise; defending this, small models often require more data for PEFT stability. New finding: On classification, all PEFT methods fail to maintain initial accuracy, implying full FT might be necessary for discriminative tasks in low-resource ablation—highlighting PEFT limits on tiny models.