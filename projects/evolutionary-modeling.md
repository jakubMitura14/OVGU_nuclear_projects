# Evolutionary 3D Geometric Modeling

This project is an advanced system for the **evolutionary improvement of 3D geometric modeling algorithms**, with a specific focus on generating "supervoxels" for medical imaging data. It leverages a Very Large Language Model (VLLM) to automate the process of finding and fixing flaws in geometric algorithms, creating a powerful feedback loop for continuous, automated code improvement.

## Core Concept: Evolutionary Algorithm Refinement

The central idea is to treat a geometric algorithm not as static code, but as an entity that can be evolved and improved. The project uses an LLM-driven, iterative refinement process rather than a classic genetic algorithm.

The workflow is as follows:
1.  **Execute**: A "seed" algorithm is run on a 3D grid of data to generate supervoxel shapes.
2.  **Validate**: Each generated shape is tested for critical geometric properties:
    *   **Watertightness**: Is the mesh a closed, solid object with no holes?
    *   **Star-Convexity**: Does the shape avoid self-intersections when viewed from its center?
3.  **Get Feedback**: If validation fails, the system packages the error report and the faulty algorithm into a prompt for the VLLM.
4.  **Refine**: The VLLM analyzes the failure and generates a new, corrected version of the algorithm.
5.  **Iterate**: This new algorithm becomes the next candidate, and the cycle repeats. This "survival of thefittest" approach allows the system to autonomously discover more robust and sophisticated algorithms.

## How the System Works: Key Components

The system is composed of several key modules:

#### 1. Custom Geometric Language
- **File**: `constrained_vllm_python/new_lark/grammar.py`
- **Purpose**: Defines a simple, domain-specific language (DSL) for 3D shape generation. The language includes commands for creating points (`svCenter`, `mob`), performing linear interpolations (`lin_p`), projecting points (`ortho_proj`), and defining triangles (`defineTriangle`). This constrained grammar makes it easier for the VLLM to generate valid and meaningful geometric logic.

#### 2. Algorithm Executor
- **File**: `constrained_vllm_python/execute_algorithm_on_data.py`
- **Purpose**: This is the engine that runs the geometric algorithms. It takes a script written in the custom DSL and executes it across a 3D grid. It uses Python's `multiprocessing` to parallelize the computation for each cell in the grid, making the process highly efficient.

#### 3. Parser and Transformer
- **File**: `constrained_vllm_python/lark_transformer.py`
- **Purpose**: Utilizes the `Lark` parsing library to read a text-based algorithm script and convert it into a structured tree. The `GeometricAlgorithmTransformer` then traverses this tree, executing the geometric operations to generate the final 3D points and triangle mesh.

#### 4. Validation and Quality Control
- **Files**: `constrained_vllm_python/is_water_tight.py`, `constrained_vllm_python/new_lark/is_line_intersect.py`
- **Purpose**: These modules contain the critical validation logic. After a shape is generated, these functions are called to ensure the mesh is a valid, well-formed polyhedron. Failures in these checks provide the necessary feedback for the evolutionary loop.

#### 5. The "Evolutionary" Engine and Prompts
- **File**: `constrained_vllm_python/prompts.py`
- **Purpose**: This file contains the `INITIAL_ALGORITHM` which serves as the starting point for the evolutionary process. It also contains the prompt templates used to communicate with the VLLM, instructing it on how to analyze and correct a failing algorithm.

## How to Run the Tests

The primary test suite for this project validates the algorithm execution and generation logic. To run the tests, execute the following command from the root of the repository:

```bash
python3 constrained_vllm_python/lark_main_test.py
```

This will:
- Run `test_full_algorithm_a`, which validates a simplified, watertight algorithm.
- Run `test_full_algorithm_b`, which tests a more complex algorithm with advanced geometric operations.
- Run `test_full_algorithm`, which verifies the parser and execution flow with predictable values.
- Perform validation checks for watertightness and star-convexity on the generated meshes.

[Back to all projects](../README.md)