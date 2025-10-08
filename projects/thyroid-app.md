# Clinical Decision Support System for Thyroid Cancer

## 1. Project Overview

This project is a sophisticated clinical decision support system designed to assist healthcare professionals in managing thyroid cancer. It leverages a knowledge graph built on Neo4j, a powerful rule generation engine powered by Large Language Models (LLMs), and a set of logic-driven Python modules to provide recommendations based on established clinical guidelines.

The core of the system is its ability to automatically translate unstructured clinical guideline text into a structured, machine-readable format. This allows for dynamic, evidence-based risk assessment, treatment advice, and monitoring suggestions tailored to individual patient data.

## 2. System Architecture

The system operates in two main phases: **Rule Generation** (an offline process) and **Inference** (a real-time process).

### 2.1. Rule Generation Workflow

This is the automated process of building the knowledge graph from clinical texts. It is orchestrated by the `src/rule_generator.py` script.

1.  **Input**: The process starts with a raw clinical guideline document (`data/more_detailed_decision_algorithm.txt`) and a set of predefined clinical concepts (decisions, variables, etc., in `data/organized_input_jsons/`).
2.  **LLM-Powered Processing**: The script systematically iterates through every possible combination of a clinical **Decision** (e.g., "Perform Total Thyroidectomy") and a clinical **Variable** (e.g., "Tumor size > 4cm").
3.  **Two-Phase Generation**: For each pair, it uses a two-phase LLM prompting strategy to discover complex relationships:
    *   **Phase 1: Validation**: It first asks the LLM to create a *simple rule* and determine if the variable is relevant to the decision (`is_relevant: true/false`).
    *   **Phase 2: Expansion**: If a relevant simple rule is found, it uses a second prompt to ask the LLM to find a *more complex, multi-conditional rule* in the text that includes the original variable plus other co-occurring conditions.
4.  **Output**: The generated rules (simple, complex, and irrelevant) are saved as structured JSON files in `data/generated_rules/`.
5.  **Graph Population**: As rules are generated, only those marked as `is_relevant: true` are used to populate a **Neo4j graph database**, creating a rich network of clinical relationships.

### 2.2. Inference Workflow

This is the real-time process of getting a recommendation for a specific patient.

1.  **Input**: A user provides a dictionary of patient findings.
2.  **Analysis**: This data is passed to one of the system's modules:
    *   **Accessor Modules**: For logic that is hard-coded based on well-defined guidelines, the data is passed to standalone modules like `src/risk_factor_accessor.py`. These provide quick, direct analysis without querying the graph.
    *   **Decision Engine**: For complex recommendations that depend on the generated rules, the data is passed to a query helper, which constructs a Cypher query.
3.  **Graph Traversal**: The Cypher query traverses the Neo4j graph, finding all rules that are satisfied by the patient's findings.
4.  **Output**: The system returns a set of recommendations, such as suggested treatments, identified risks, or tests to order.

## 3. Core Components & File Guide

This section describes the purpose of the key files and directories in the project.

| Path                                  | Description                                                                                                                                                                                               |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `run_generation_slurm_nemotron.sh`    | **(Primary HPC Script)** The main job submission script for running rule generation on a **SLURM cluster with the Nemotron model**. It launches the Apptainer container and the second script.           |
| `run_vllm_and_generator.sh`           | **(Container Script)** This script runs *inside* the Apptainer container. It starts the vLLM server, waits for it to be ready, and then executes the Python rule generator.                               |
| `run_generation.sh`                   | The runner script for running the rule generation process on a **local machine**. It automatically detects GPUs (for the MedGemma model) and includes a `--force-remote` flag.                         |
| `environment.yml`                     | The Conda environment definition file. Specifies all Python and CUDA dependencies required to run the project, ensuring a reproducible environment, especially for the SLURM jobs.                       |
| `src/rule_generator.py`               | The core script for the rule generation workflow. It contains the logic for the two-phase LLM prompting, asynchronous processing, and populating the Neo4j database.                                        |
| `src/neo4j_connector.py`              | A utility class that manages the connection to the Neo4j graph database.                                                                                                                                  |
| `data/`                               | Contains all data used by the system.                                                                                                                                                                     |
| `data/more_detailed_decision_algorithm.txt` | The primary source text of the clinical guidelines used by the LLM for rule generation.                                                                                                                   |
| `data/organized_input_jsons/`         | Contains JSON files that define the structured clinical concepts of the project, such as all possible decisions, patient types, and variables.                                                            |
| `data/generated_rules/`               | The output directory where the `rule_generator.py` script saves all the JSON rules it generates.                                                                                                          |
| `how_rules.md`                        | **(Important)** A detailed guide explaining the structure of the JSON rules created by the `rule_generator.py` script. **Read this to understand the output.**                                           |

## 4. Execution Guide

Follow these steps to set up and run the rule generation process.

### 4.1. Prerequisites

-   **Conda**: You must have Conda or Miniforge installed.
-   **NVIDIA GPU**: A GPU is required for running with a local vLLM server (either on a local machine or on the HPC cluster).
-   **Hugging Face Token**: To download gated models like MedGemma or Nemotron, you need a Hugging Face account with access granted to the model and a corresponding authentication token.

### 4.2. HPC Execution (SLURM + Nemotron)

This is the primary intended method for large-scale rule generation.

1.  **Place Project on Cluster**: Ensure the entire project directory (`tumor_board_process`) is located at `/mnt/vast-kisski/projects/ovgu_medicine_llm/tumor_board_process`.
2.  **Submit the Job**: From the project's root directory, submit the SLURM script using `sbatch`.
    ```bash
    sbatch run_generation_slurm_nemotron.sh
    ```
    The script handles everything else automatically: requesting resources, setting up the environment inside the container, managing the vLLM server, and running the generation process. All outputs and logs will be saved to `/mnt/vast-kisski/projects/ovgu_medicine_llm/ollama_data/tumor_board/data/temp`.

### 4.3. Local Execution (MedGemma Model)

#### With a GPU (Recommended)

1.  **Create Conda Environment**:
    ```bash
    conda env create -f environment.yml
    conda activate clinical-rules-env
    ```
2.  **Set Hugging Face Token**:
    ```bash
    export HUGGING_FACE_HUB_TOKEN="hf_your_token_here"
    ```
3.  **Run the Script**:
    ```bash
    bash run_generation.sh
    ```
    The script will automatically detect your GPU, start a local vLLM server with the MedGemma model, and run the Python script against it.

#### Without a GPU (Using Remote Server)

1.  **Configure `.env` file**: Create a `.env` file in the root directory with the credentials for the remote LLM and Neo4j database.
    ```
    API_KEY="your_remote_api_key"
    API_BASE_URL="https://your.remote.server/v1"
    MODEL_NAME="model-on-remote-server"
    NEO4J_URI="..."
    NEO4J_USERNAME="..."
    NEO4J_PASSWORD="..."
    ```
2.  **Run the Script with `--force-remote`**:
    ```bash
    bash run_generation.sh --force-remote
    ```
    This flag tells the script to skip the GPU check. The Python script will then load the credentials from your `.env` file and run with a concurrency of 1.

## 5. Detailed Documentation Links

For a deeper dive into specific components of the system, please refer to the following documents:

| File                                | Description                                                                                                                              |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| [how_rules.md](how_rules.md)        | **(Recommended Reading)** Explains the structure and meaning of the automatically generated JSON rules.                                  |
| [ACCESSORS_EXPLAINER.md](ACCESSORS_EXPLAINER.md) | Explains the purpose and usage of the standalone data accessor modules (`RiskFactorAccessor`, etc.).                                     |
| [AND_OR_RULE_DOCUMENTATION.md](AND_OR_RULE_DOCUMENTATION.md) | Details the JSON and Neo4j schema for the simple "AND" and complex "AND/OR" rules that power the decision engine.                      |
| [neo4j_schema.md](neo4j_schema.md)  | Outlines the complete Neo4j graph schema, including all node labels, properties, and relationship types.                                 |
| [DETAILED_VALIDATION_REPORT.md](DETAILED_VALIDATION_REPORT.md) | A comprehensive report validating the system's logic against numerous clinical scenarios to ensure its accuracy and reliability. |

[Back to all projects](../README.md)