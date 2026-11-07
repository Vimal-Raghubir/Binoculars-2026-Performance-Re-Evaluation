# Binoculars 2026 re-evaluation

Does Binoculars, a well-known zero-shot AI text detector, still catch AI text from 2026 models? It was benchmarked on the models of 2023 and early 2024.

I tested it on four groups of text: human texts from the RAID benchmark, older AI texts from RAID (GPT-4, Mistral chat), new texts from the 2026 successors (gpt-6-luna, Mistral Large 4) written from the same RAID prompts, and those 2026 texts paraphrased by the other model.

**Post:** [I tested a top AI detector on 2026 models. Here's how much it missed.](SUBSTACK_LINK)
**Notebook:** [Kaggle](https://www.kaggle.com/code/vraghubir/binoculars-2026-performance-re-evaluation)
**Data:** [Kaggle datasets](DATASETS_LINK)

## Results

Strict "low false-flag" threshold (0.8536), full texts:

| Group | n | Flagged as AI |
|---|---|---|
| Human | 200 | 0.5% (wrongly flagged) |
| Older AI (GPT-4, Mistral chat) | 200 | 76.5% |
| 2026 AI (gpt-6-luna, Mistral Large 4) | 199 | 19.6% |
| 2026 AI, paraphrased | 100 | 6.0% |

By family: Mistral fell from 99% to 12%, and GPT from 54% to 27%. Much of GPT's drop comes from gpt-6-luna writing shorter texts. When every text is cut to its first 120 words, GPT goes from 26% to 21%, while Mistral still falls from 97% to 8%.

This is a small test: about 200 texts per group, 2 domains (news and reviews), and 1 detector. Treat the numbers as a direction, not exact rates.

## How to run it

The notebook is built to run on Kaggle.

| Option | What you need | Time |
|---|---|---|
| **Use the saved data** (reproduces my results) | Attach the Kaggle datasets as input and Run All. Every step loads its saved file and skips its work. | A few minutes, CPU only |
| **Full rerun** | `OPENAI_API_KEY` and `MISTRAL_API_KEY` in Kaggle Secrets, GPU T4 x2, a few dollars of API credit | About 2 hours of GPU time |

A full rerun generates new 2026 texts, so your numbers will differ slightly from mine.

Setup notes:
- Binoculars is installed from the [official repo](https://github.com/ahans30/Binoculars) with `pip install --no-deps`.
- Pin `transformers==4.57.6`, because version 5 breaks the Falcon models Binoculars uses. Restart the kernel after installing.
- Never paste API keys into a cell. Use Kaggle Secrets.

## Steps

1. Install and set up
2. Configure paths, and load saved data if it's there
3. Sample 200 human and 200 older AI texts from RAID `train_none` (news and reviews)
4. Generate 2026 texts from the same prompts
5. Paraphrase 100 of them with the other 2026 model
6. Score everything with Binoculars (full text and first 120 words)
7. Results tables with 95% Wilson intervals
8. Chart
9. Look at the biggest misses and the score distributions

## Credits

- **Binoculars:** Hans et al. (2024), *Spotting LLMs With Binoculars: Zero-Shot Detection of Machine-Generated Text*. [Code](https://github.com/ahans30/Binoculars) (BSD-3-Clause). Not included in this repo; it's installed at runtime.
- **RAID:** Dugan et al. (2024), *RAID: A Shared Benchmark for Robust Evaluation of Machine-Generated Text Detectors*. [Code and data](https://github.com/liamdugan/raid) (MIT). The human and older AI texts come from RAID.

## License

MIT. See `LICENSE`.
