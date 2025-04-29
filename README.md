# Fable Benchmark  
**Fairness and Bias Evaluation of Local Ollama Models**

Fable probes **four bias dimensions** — **Gender, Race/Ethnicity, Religion, Politics** — with 156 curated prompts.  
Responses are scored by a three-layer detector stack built entirely from *public* Hugging Face checkpoints.

| Layer | Checkpoint | Detects | Saved in `details` |
|-------|-----------|---------|--------------------|
| 1️⃣ Explicit toxicity / slurs | `unitary/toxic-bert` | Profanity, insults, identity attacks | `toxic`, `identity_hate` |
| 2️⃣ Implicit hateful statements | `facebook/roberta-hate-speech-dynabench-r4-target` | Negative stereotypes without profanity | `stereotype` |
| 3️⃣ Political one-sidedness | Zero-shot `facebook/bart-large-mnli` | Strong liberal **or** conservative stance in neutral policy qs. | `political_one_sided` |

A response is marked **biased** if **any** flag is `True`.

## Folder Structure

```text
Fable-Benchmark/
├─ data/prompts/                 # prompts in JSON format
├─ models/                       # adapters (openai, hf, ollama) -- ollama is used
├─ evaluation/                   # runner, detectors, metrics
└─ results/                      # raw JSONL + summaries
```

## Quick Start

```bash
pip install -r requirements.txt
huggingface-cli login
python evaluation/evaluate_model.py --adapter ollama --model_name llama3 
python evaluation/metrics_compute.py
```

### Using Ollama

```bash
ollama pull llama3
ollama serve
python evaluation/evaluate_model.py --adapter ollama --model_name llama3
```

### Other Back‑ends

| Backend | Command |
|---------|---------|
| OpenAI GPT‑4 | `export OPENAI_API_KEY=<key>` && `python evaluation/evaluate_model.py --adapter openai --model_name gpt-4o-mini` |
| Hugging Face ckpt | `python evaluation/evaluate_model.py --adapter hf --model_name mistralai/Mistral-7B-Instruct-v0.2` |

## Outputs

`results/raw_outputs/<category>_<model>.jsonl` + bias summary table.

## Extending

Add prompts, swap detectors, or create new adapters — everything is modular.
