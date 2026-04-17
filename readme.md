AnchorMem: Anchored Facts with Associative Contexts for Building
Memory in Large Language Models ⚓🧠
===================================

## Introduction 🌟

Our introduces **AnchorMem**, a novel memory architecture that organizes information into a two-layer hierarchy: Facts and Events. By clustering related atomic facts and summarizing them into Event, AnchorMem enables more precise retrieval and better reasoning for agentic tasks. Experiments across three closed-source and open-source models with **difference methods (e.g., HippoRAG 2, LightMem)** on the LoCoMo benchmark demonstrate that AnchorMem significantly outperforms baselines.

<div align="center">
    <img src="figure/first_figure_anchormem.jpg" width="800"/>
    <br>
    <em>Comparison between traditional memory systems and our AnchorMem architecture. AnchorMem abstracts discrete facts into event-level anchors for superior retrieval.</em>
</div>

## New 🌟
- [2026/04/6] 🔥 Our paper has been accepted to **ACL 2026** Finding!
  
## Key Features ✨

- 🔄 **Hierarchical Memory Organization**: Automatically extracts atomic "Facts" and abstracts them into high-level "Events".
- 🔍 **Semantic Fact Grouping**: Uses vector similarity to cluster related facts from different time steps before summarizing them into events.
- ⚡ **RAG-Optimized Retrieval**: Dual-path retrieval that matches both facts and events to provide comprehensive context for QA.
- 📊 **Comprehensive Evaluation Pipeline**: Includes standard metrics (Recall, EM, F1, Bleu) and an advanced **LLM-as-a-judge** evaluation flow.
- 📦 **Lightweight Embedding Store**: Built-in persistent storage for embeddings and metadata without needing external database dependencies.

## Framework 🏗️

<div align="center">
<img src="figure/method_AnchorMem.jpg" alt="AnchorMem Framework" width="800"/>

<em>The AnchorMem framework: From document chunking and fact extraction to event anchoring and retrieval-augmented generation.\</em>
</div>

## How It Works 🛠️

1. **Fact Extraction**: The system parses documents or dialogues into chunks and uses an LLM to extract discrete, atomic facts.
2. **Clustering & Linking**: Extracted facts are embedded and compared. Facts with high semantic similarity are grouped together.
3. **Event Abstraction**: An LLM analyzes these "fact groups" to generate high-level event descriptions, creating "anchors" between levels.
4. **Retrieval-Augmented QA**: During a query, the system retrieves the most relevant facts and events to construct a rich context for the final answer.

***

## Getting Started 🚀

### 1. Installation

Clone the repository and install the dependencies listed in `requirement.txt`:

```bash
git clone https://github.com/your-username/AnchorMem.git
cd AnchorMem

# Create a virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirement.txt
```

### 2. Basic Usage (Integrating AnchorMem) 💡

Here is how to use AnchorMem as the memory system for your own agent:

```python
from AnchorMem import AnchorMem
from src.utils.config_utils import BaseConfig

# Initialize Configuration 🚀
config = BaseConfig(
    llm_base_url="http://localhost:8080/v1",
    llm_name="Qwen2.5-7B",
    embedding_model_name="Transformers/all-MiniLM-L6-v2",
    save_dir="outputs/my_experiment"
)

# Initialize the Memory System
memory = AnchorMem(global_config=config)

# 1. Add Memories (Indexing) ➕
docs = [
    "John visited Paris in June and saw the Eiffel Tower.",
    "While in Paris, John met an old friend named Sarah.",
    "Sarah gave John a vintage book as a gift."
]
memory.index(docs)

# 2. Query the Memory (RAG QA) 📖
queries = ["What did Sarah give to John?"]
solutions, responses, metadata = memory.rag_qa(queries)

for sol in solutions:
    print(f"Question: {sol.question}")
    print(f"Answer: {sol.answer}")
    print(f"Top Facts Found: {sol.topk_facts}")
```

***

## Evaluation Pipeline 📊

### Running the Main Benchmark

Use `main.py` to run the retrieval and QA pipeline on datasets like LoCoMo:

```bash
python main.py \
    --dataset locomo \
    --dataset_path data/locomo10.json \
    --llm_name Qwen2.5-7B \
    --llm_base_url http://localhost:8080/v1 \
    --save_dir outputs
```

### Running LLM-as-a-Judge

After running the main pipeline, use `eval_judge.py` to perform semantic accuracy evaluation using a stronger model:

```bash
python eval_judge.py \
    --dataset locomo \
    --base_save_dir outputs \
    --llm_judge_model Qwen2.5-32B \
    --llm_base_url http://localhost:8080/v1 \
    --concurrency 20
```

## Citation 📚

If you use AnchorMem in your research, please cite our work:

```bibtex
@article{anchormem2026,
  title={AnchorMem: Anchored Facts with Associative Contexts for Building Memory in Large Language Models},
  author={Zhanyu Shen, Sijie Cheng, Zhicheng Guo, Weiqin Wang, Yile Wang, Hui Huang },
  year={2026}
}
```

## License 📄

This project is licensed under the MIT License. See `LICENSE` for details.
