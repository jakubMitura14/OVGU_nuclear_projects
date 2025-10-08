# Evolutionary 3D Geometric Modeling: AI That Improves Itself

This project showcases a groundbreaking approach to **improving 3D geometric modeling algorithms through an evolutionary, AI-driven process**. With a specific focus on generating high-quality "supervoxels" for medical imaging, this system leverages a Very Large Language Model (VLLM) to automate the discovery and correction of flaws in geometric code, creating a powerful feedback loop for continuous, automated improvement.

## Core Concept: Evolutionary Algorithm Refinement

The central innovation is to treat a geometric algorithm not as static code, but as an entity that can be **evolved and refined by an AI**. Instead of manual debugging, the system uses an LLM-driven iterative process:

1.  **Execute & Validate**: A "seed" algorithm runs to generate 3D supervoxel shapes from medical data. Each shape is then rigorously tested for critical geometric properties like **watertightness** (no holes) and **star-convexity** (no self-intersections).
2.  **AI-Driven Feedback & Refinement**: If validation fails, the system automatically packages the error report and the faulty algorithm into a prompt for the VLLM. The AI analyzes the failure and generates a new, corrected version of the algorithm.
3.  **Iterate & Evolve**: This new algorithm becomes the next candidate, and the cycle repeats. This "survival of the fittest" approach allows the system to autonomously discover more robust and sophisticated algorithms, far beyond what could be easily designed by hand.

This demonstrates a novel paradigm in which an AI doesn't just execute a task but actively participates in the **creative process of algorithm design**.

## Key Innovations & Expertise

The system's architecture highlights our expertise in combining classical computer science with cutting-edge AI:

-   **Custom Geometric Language**: We designed a simple, domain-specific language (DSL) for 3D shape generation. This constrained grammar makes it easier for the VLLM to generate valid and meaningful geometric logic, bridging the gap between natural language and precise geometric operations.
-   **High-Performance Execution Engine**: The system uses a highly efficient engine to run the geometric algorithms in parallel across a 3D grid, leveraging Python's `multiprocessing` capabilities to handle large medical imaging datasets.
-   **Automated Geometric Validation**: Sophisticated validation modules automatically check for complex geometric properties. These checks provide the critical feedback signal that drives the evolutionary process, ensuring that only robust, high-quality algorithms survive.
-   **The "Evolutionary" Engine**: The core of the project is the prompt engineering and logic that enables the VLLM to understand, debug, and rewrite its own geometric code. This showcases a deep understanding of how to guide and constrain LLMs for complex, creative problem-solving tasks.

This project represents a significant step towards **self-improving systems** in the field of medical imaging, where the quality and robustness of geometric algorithms are paramount.

[Back to all projects](../README.md)