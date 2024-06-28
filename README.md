# Revolutionizing Text-to-Image Models with LLM-Grounded Diffusion

![x2](https://github.com/rbhardwaj2186/LLM_Diffusion_Model/assets/143745073/ea0ade5d-a862-41da-b568-f1212c7b49aa)



## Features of Our Repository

    SDXL Refiner: For generating high-resolution, high-quality images.
    LLM Query Caching: Save on API costs by caching each query for layout generation.
    Open-source LLM Support: Host models yourself for flexibility and cost-efficiency (supports Vicuna, LLaMA 2, StableBeluga2, etc.).
    Support for SD v1 and v2: New features and losses likely work on both versions.
    Baseline Methods: Multiple stage 2 methods implemented for benchmarking and comparison.
    Hackable Architecture: Minimal diffusion UNet implementation for custom modifications.
    Parallel & Resumable Generation: Utilize multiple GPUs/servers and handle failed generations gracefully.
    Modular Design: Methods are implemented in separate files for ease of customization.
    Web UI: Public WebUI demo and local instructions for faster, queue-free generation.

## Methods Implemented

    Training-Free LMD: Utilizing original SD v1/v2 weights.
    LMD+: Incorporates cross-attention control and GLIGEN adapters.
    Layout Guidance (Backward Guidance)
    BoxDiff (ICCV '23)
    MultiDiffusion (Region Control, ICML '23)
    GLIGEN (CVPR '23)

## Installation and Usage
Installation

bash

```pip install -r requirements.txt```

Stage 1: Text-to-Layout Generation

Cached layouts are available; remove cache if re-generation is needed. Layout format includes captioned boxes, background prompt, and negative prompt. Templates and examples are in prompt.py.

Automated Method (OpenAI API Key Required):

bash

```python prompt_batch.py --prompt-type demo --model gpt-4 --auto-query --always-save --template_version v0.1```

Manual Method (Using ChatGPT):

bash

```python prompt_batch.py --prompt-type demo --model gpt-4 --always-save --template_version v0.1```

Stage 2: Layout-to-Image Generation

No need to run stage 1 for provided cached prompts. Run layout-to-image generation with the following command:

bash

```python generate.py --prompt-type demo --model gpt-4 --save-suffix "gpt-4" --repeats 5 --frozen_step_ratio 0.5 --regenerate 1 --force_run_ind 0 --run-model lmd_plus --no-scale-boxes-default --template_version v0.1```

Benchmark Evaluation

Use the following commands for benchmarking and evaluation:

bash

```python prompt_batch.py --prompt-type lmd --model gpt-3.5 --auto-query --always-save --template_version v0.1```
```python scripts/eval_stage_one.py --prompt-type lmd --model gpt-3.5 --template_version v0.1```

Frequently Asked Questions
Using Open-Source LLMs

To host LLMs like Mixtral, LLaMA-2, StableBeluga2, or Vicuna, install fastchat and follow these steps:

Setup:

bash

```pip install fschat```

```export FASTCHAT_WORKER_API_TIMEOUT=600```
# Terminal 1
```python3 -m fastchat.serve.controller```
# Terminal 2
```CUDA_VISIBLE_DEVICES=0,1 python3 -m fastchat.serve.model_worker --model-path mistralai/Mixtral-8x7B-Instruct-v0.1 --num-gpus 2 --max-gpu-memory 48GiB```
# Terminal 3
```python3 -m fastchat.serve.openai_api_server --host localhost --port 8000```

    
