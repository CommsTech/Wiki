---
title: GPT4all Finetining
description: A comprehensive note in my Obsidian vault about fine-tuning the GPT4ALL model – a large language model that can be fine-tuned on customized local data to improve its performance and accuracy for specific tasks. This note may include information on benefits, considerations, steps involved, and best practices for fine-tuning the GPT4ALL model using your well-prepared A-Shares stock market data or other specialized datasets.
dateCreated: 2023-12-30T02:41:39.777Z
published: true
editor: markdown
tags:
  - GPT4ALL
  - Fine_Tuning
  - Machine_Learning
dateModified: 
---
# GPT4ALL: Train with local data for Fine-tuning

Fine-tuning large language models like GPT (Generative Pre-trained Transformer) has revolutionized natural language processing tasks. While pre-training on massive amounts of data enables these models to learn general language patterns, fine-tuning with specific data can further enhance their performance on specialized tasks.

This article explores the process of training with customized local data for GPT4ALL model fine-tuning, highlighting the benefits, considerations, and steps involved.

Fine-tuning with customized local data allows GPT models to leverage domain-specific knowledge, resulting in better performance and more accurate outputs for specific tasks. For my use case, I’ve got well-prepared A-Shares stock market data(for Openai fine-tuning purposes originally). It seems these datasets can be transferred to train a GPT4ALL model as well with some minor tuning of the code.

# model/tokenizer  
model_name: "decapoda-research/llama-7b-hf" # add model here  
tokenizer_name: "gpt2" # add model here  
gradient_checkpointing: true  
save_name: "save_folder/gpt4all_str2plusallti" # CHANGE   
  
# dataset  
streaming: false  
num_proc: 32  
dataset_path: "configs/deepspeed/tbldailyAShareSTR2Plus_RAWSUM_forGPT10percentTestTI_prepared_train.jsonl" # update  
max_length: 1024  
batch_size: 32  
  
# train dynamics  
lr: 5.0e-5  
eval_every: 800  
eval_steps: 100  
save_every: 800  
output_dir: "model_data/gpt4all_str2plusallti" # CHANGE  
checkpoint: null  
lora: false  
warmup_steps: 100  
num_epochs: 2  
  
# logging  
wandb: true  
wandb_entity: "maxtensor" # update  
wandb_project_name: "gpt4all_str2plusallti" # update  
seed: 42

# A minor twist on GPT4ALL and datasets package

The Huggingface datasets package is a powerful library developed by Hugging Face, an AI research company specializing in natural language processing (NLP) technologies. It aims to simplify and streamline the process of working with datasets in NLP tasks. The package provides a comprehensive collection of datasets, including popular benchmarks, corpora, and specialized datasets, along with a unified API for easy access and manipulation.

Name: datasets  
Version: 2.13.0  
Summary: HuggingFace community-driven open-source library of datasets  
Home-page: https://github.com/huggingface/datasets  
Author: HuggingFace Inc.  
Author-email: thomas@huggingface.co  
License: Apache 2.0  
Location:   
Requires: aiohttp, dill, fsspec, huggingface-hub, multiprocess, numpy, packaging, pandas, pyarrow, pyyaml, requests, tqdm, xxhash  
Required-by: evaluate

If you have some experience with Openai Fine-tuning, the .jsonl dataset prepared conclude only two columns, “prompt” and “completion”. After running the following command in the terminal as mentioned by Cocobeach, I encountered a ValueError indicating a missing column in my datasets:

Traceback (most recent call last):  
  File "C:\Users\zhoub\git_repo\gpt4all\gpt4all-training\train.py", line 234, in <module>  
    train(accelerator, config=config)  
  File "C:\Users\zhoub\git_repo\gpt4all\gpt4all-training\train.py", line 53, in train  
    train_dataloader, val_dataloader = load_data(config, tokenizer)  
  File "C:\Users\zhoub\git_repo\gpt4all\gpt4all-training\data.py", line 89, in load_data  
    train_dataset = train_dataset.map(  
  File "C:\Users\zhoub\.conda\envs\gpt4all\lib\site-packages\datasets\arrow_dataset.py", line 580, in wrapper  
    out: Union["Dataset", "DatasetDict"] = func(self, *args, **kwargs)  
  File "C:\Users\zhoub\.conda\envs\gpt4all\lib\site-packages\datasets\arrow_dataset.py", line 545, in wrapper  
    out: Union["Dataset", "DatasetDict"] = func(self, *args, **kwargs)  
  File "C:\Users\zhoub\.conda\envs\gpt4all\lib\site-packages\datasets\arrow_dataset.py", line 3005, in map  
    raise ValueError(  
ValueError: Column to remove ['source'] not in the dataset. Current columns in the dataset: ['prompt', 'completion']

Just to make the pipe going, I comment out the error-raising part in datasets\arrow_dataset.py.

if remove_columns is not None and any(col not in self._data.column_names for col in remove_columns):  
  #test  
  #raise ValueError(  
  #    f"Column to remove {list(filter(lambda col: col not in self._data.column_names, remove_columns))} not in the dataset. Current columns in the dataset: {self._data.column_names}"  
  #)  
  pass

Due to the Openai fine-tuning format requirement, another twist I made is to change the default “response” column name with “completion” in GPT4ALL’s data.py script, so that I won’t need to amend the dataset itself.

#test: Replace column name "response" with "completion".  
for prompt, completion in zip(examples["prompt"], examples["completion"]):  
    if different_eos:  
        if completion.count("</s> \n") > 0:  
            completion = completion.replace("</s> \n", f"{tokenizer.eos_token} \n")   
  
    prompt_len = len(tokenizer(prompt + "\n", return_tensors="pt")["input_ids"][0])  
  
    # hack if our prompt is super long  
    # we need to include some labels so we arbitrarily trunacate at max_length // 2  
    # if the length is too long  
    if prompt_len >= max_length // 2:  
        # if prompt is too long, truncate  
        # but make sure to truncate to at max 1024 tokens  
        new_len = min(max_length // 2, len(prompt) // 2)  
        prompt = prompt[:new_len]  
        # get new prompt length  
        prompt_len = tokenizer(prompt + "\n", return_tensors="pt", max_length=max_length // 2, truncation=True).input_ids.ne(tokenizer.pad_token_id).sum().item()  
  
    assert prompt_len <= max_length // 2, f"prompt length {prompt_len} exceeds max length {max_length}"  
  
    input_tokens = tokenizer(prompt + "\n" + completion + tokenizer.eos_token,  
                             truncation=True, max_length=max_length, return_tensors="pt")["input_ids"].squeeze()  
  
    labels = input_tokens.clone()  
    labels[:prompt_len] = -100  
    if len(labels) < max_length:  
        # pad to max_length with -100  
        labels = torch.cat([labels, torch.full((max_length - len(labels),), -100)])  
  
    assert (labels == -100).sum() < len(labels), f"Labels are all -100, something wrong. prompt length {prompt_len} exceeds max length {max_length}"   
      
    if (labels == -100).sum() == len(labels) - 1:  
        print(prompt)  
        print(completion)  
        raise

Another problem regarding train.py seems to be caused by the different locations of modules DummyScheduler, DummyOptim, set_seed.

The versions I’m using is accelerate==0.20.3 and transformers==4.29.0 and below fix works for me under Python3.9.0;

from accelerate.utils.deepspeed import DummyScheduler, DummyOptim  
from accelerate.utils.random import set_seed  

Then, launch the fine-tuning with this terminal command in Cocobeach’s post:

accelerate launch --num_processes=8 --num_machines=1 --machine_rank=0 train.py --config configs/train/replacewithyourfilename.yaml

![](https://miro.medium.com/v2/resize:fit:1000/1*LvQaSVl9VAA7GaHSVkEewQ.png)