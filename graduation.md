# Graduation Project: AI_router (2024.01 - 2024.12)

Repository: JMTdongsan/AI_router

## Summary
- Built a RAG-based QA system using Milvus + vLLM with Flask APIs, tools, and crawling.
- For content not included in the repository pipeline, I served models directly with vLLM.
- Researched GPU parallel inference methods, quantization, and efficient inference techniques.

## Optimization case study
During the real estate chatbot graduation project, I needed local serving of Qwen-72B to reduce API cost while keeping high inference performance. It required about 180GB VRAM.
Since H100-class GPUs were unavailable, I built a multi-consumer-GPU setup and optimized both hardware and software. I removed multi-GPU communication bottlenecks by improving NVLink usage and rebalancing PCIe lanes to raise bandwidth efficiency.
On the software side, I evaluated quantization methods and analyzed 4-bit model collapse and the low throughput of GGUF. I chose vLLM-based GPTQ INT8 to balance quality and speed, and I optimized VRAM usage to keep as much context as possible.
This experience taught me that GPU interconnect tuning and memory techniques like PagedAttention are decisive factors for inference performance when serving large models under tight resource constraints.
