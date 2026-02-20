# Graduation Project: AI_router (2024.01 - 2024.12)

Repository: JMTdongsan/AI_router

## Summary
- Built a RAG-based QA system using Milvus + vLLM with Flask APIs, tools, and crawling.
- For content not included in the repository pipeline, I served models directly with vLLM.
- Researched GPU parallel inference methods, quantization, and efficient inference techniques.

## Architecture for reliability
I structured the system as an agent-like pipeline with explicit function-call steps to reduce hallucinations and maximize answer reliability.
1) RAG with a vector DB
2) RAG with a search engine
3) Answer rejection step when evidence is insufficient

## Data preprocessing
- Chunking was performed by an LLM to preserve semantic boundaries.
- For embedding accuracy, an LLM extracted only the core content to embed instead of full pages.

## Cost analysis and on-prem decision
We could have used a paid API, but GPT 4 32K cost $120 per 1M tokens. With about 500 pages per year, each page averaging 2,000 characters and ~2 tokens per character(becase of korean), preprocessing required roughly 3M tokens after a 50% overlap rate during chunking.
Seoul Metropolitan Government redevelopment documents cover 13 sectors: overall city direction, urban maintenance (7 types), residential environment improvement, urban regeneration, roads and streets, and parks/green spaces (2 types).
This expanded to about 40M tokens per update every 6 months. At $120 * 40, this was roughly 6 million KRW at the exchange rate at the time, so I decided to build a local serving stack.

## Optimization case study
During the real estate chatbot graduation project, I needed local serving of Qwen-72B to reduce API cost while keeping high inference performance. It required about 180GB VRAM.

### Hardware optimization
- Replaced an H100-class target with 4x RTX 3090s for local serving.
- Reduced multi-GPU communication bottlenecks by improving NVLink usage and rebalancing PCIe lanes to raise bandwidth efficiency.
- Consumer CPUs are limited to 24 PCIe lanes. I reduced GPU P2P bottlenecks by using a motherboard supporting 8+8+8+4 lanes and leveraging NVLink.
- The initial 16+4+4+4 configuration showed GPU P2P as the bottleneck, so I procured a board with PCIe bifurcation support.
- Throughput increased by about 80% (100 concurrency), rising from 220-280 tps to 420 tps.

### Serving lightweight optimization
- Evaluated quantization methods and analyzed 4-bit model collapse and the low throughput of GGUF.
- Verified stability for CoT and function-call usage to avoid langfusion or model collapse at higher quantization levels.
- Chose vLLM-based GPTQ INT8 to balance throughput and stability.

### Memory optimization
- Optimized VRAM usage to keep as much context as possible.
- Selected vLLM with KV cache quantization and PagedAttention to maximize context length.
- Avoided CUDA graphs to prevent extra memory consumption.
- Used tensor parallel instead of pipeline parallel because pipeline parallel consumed more VRAM and reduced context (PP likely stores activation values).

## Lessons learned
GPU interconnect tuning and memory techniques like PagedAttention are decisive factors for inference performance when serving large models under tight resource constraints.
