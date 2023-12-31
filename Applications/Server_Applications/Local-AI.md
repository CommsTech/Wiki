---
title:
description:
published:
aliases: 
tags: 
---
# Local-AI
curl http://localhost:8080/models/available | jq '.[] | select(.name | contains("llama2"))'

curl http://localhost:8080/models/apply -H "Content-Type: application/json" -d '{
    "id": "huggingface@thebloke__luna-ai-llama2-uncensored-ggml__luna-ai-llama2-uncensored.ggmlv3.q4_k_s.bin",
    "name": "llama-2-uncensored-q4ks"
	}'

curl http://localhost:8080/models/jobs/cfeb114d-615e-11ee-83d0-660b5025861c


wget https://gpt4all.io/models/ggml-gpt4all-j.bin -O /models/ggml-gpt4all-j

GALLERIES='[{"name":"model-gallery", "url":"github:go-skynet/model-gallery/index.yaml"}, {"url": "github:go-skynet/model-gallery/huggingface.yaml","name":"huggingface"}]' ./local-ai --models-path /models/ --debug --cors




curl http://localhost:8080/v1/chat/completions -H "Content-Type: application/json" -d '{
     "model": "llama-2-uncensored-q4ks",
     "messages": [{"role": "user", "content": "How are you?"}],
     "temperature": 0.9
   }'