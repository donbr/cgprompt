# AGENTiGraph & Graphusion Implementation Guide

## Overview

This repository contains the Graphusion pipeline described in the AGENTiGraph paper's appendix D.2. While the paper describes a full system with a dual-mode interface, this implementation focuses on the knowledge graph construction pipeline.

## Repository Contents

1. Knowledge Graph Construction Pipeline:
   - Step 1: Concept extraction (step_01_concept_extraction.py)
   - Step 2: Triple extraction (step_02_triple_extraction.py)
   - Step 3: Knowledge fusion (step_03_fusion.py)

2. Key Input Files:
   - Relation definitions: data/test/relation_types.json
   - Gold concepts: data/test/gold_concepts.tsv
   - Refined concepts: data/test/refined_concepts.tsv

## Setup Instructions

1. Create and activate Python virtual environment:
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows use `.venv\Scripts\activate`
```

2. Install requirements:
```bash
pip install -r requirements.txt
```

3. Set OpenAI API key:
```bash
export OPENAI_API_KEY="your-key-here"
```

## Running the Pipeline

1. Basic pipeline execution:
```bash
python main.py --run_name "test" --dataset "test" \
    --relation_definitions_file "data/test/relation_types.json" \
    --gold_concept_file "data/test/gold_concepts.tsv" \
    --refined_concepts_file "data/test/refined_concepts.tsv"
```

2. Pipeline outputs (in output/[run_name]/):
   - concept_abstracts.json: Concept to abstract mappings
   - concepts.tsv: Extracted concepts
   - step-02.jsonl: Initial triple extractions
   - step-03.jsonl: Refined/fused triples

## Neo4j Integration

The step-03.jsonl file contains triples that can be loaded into Neo4j to create the knowledge graph. The paper describes using this graph with a dual-mode interface (chatbot and exploration views), but that interface implementation is not included in this repository.

## Understanding the Pipeline

1. Concept Extraction (Step 1):
   - Uses BERTopic for concept discovery
   - Integrates with predefined gold concepts
   - Outputs concepts and their associated abstracts

2. Triple Extraction (Step 2):
   - Uses LLM to extract relationships between concepts
   - Follows predefined relation types from relation_definitions.json
   - Generates candidate triples

3. Knowledge Fusion (Step 3):
   - Refines and integrates triples
   - Produces final knowledge graph structure
   - Outputs step-03.jsonl for Neo4j import

## Relationship to AGENTiGraph Paper

While the paper describes a complete system with:
- 7 specialized agents
- Dual-mode user interface
- Chat and exploration capabilities

This repository implements the underlying knowledge graph construction pipeline (Graphusion) that would support such a system. The UI and agent implementations are not included.

## Available Test Files

The repository includes Jupyter notebooks for testing different components:
- test_fusion.ipynb: Tests knowledge fusion
- test_graph.ipynb: Tests graph operations
- test_topic_extraction.ipynb: Tests concept extraction
- test_tpextraction.ipynb: Tests triple extraction

These can be useful for understanding and debugging the pipeline components.

## Next Steps

If you want to build the full AGENTiGraph system:
1. Use this pipeline to construct your knowledge graph
2. Implement the 7 agents described in the paper
3. Build the dual-mode interface following the paper's specifications
4. Connect everything using the Neo4j database as the central knowledge store

## References

- Paper: "AGENTiGraph: An Interactive Knowledge Graph Platform for LLM-based Chatbots Utilizing Private Data"
- Architecture diagram: fig_architecture.png
- YouTube explanation: discoverai-yt-summary.md