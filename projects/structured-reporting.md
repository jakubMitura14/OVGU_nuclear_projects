# German Radiology Report Analyzer

This project is a sophisticated toolkit for **analyzing and structuring German-language radiology reports**. It leverages the power of Large Language Models (LLMs) to transform complex, unstructured medical texts into a standardized, validated, and machine-readable format. The primary goal is to automate the extraction of key information from these reports, making the data more accessible for clinical research, data analysis, and other applications.

The system is designed to be robust and scalable, with a multi-stage pipeline that includes translation, granular data extraction, and a unique, automated quality control loop.

## Key Innovations & Expertise

This project showcases our expertise in building reliable, high-precision data extraction pipelines for complex medical documents.

### 1. Advanced "Detect then Extract" Process
For complex, repeating entities like nodules or metastases, the system uses a highly accurate two-step process:
1.  **Detection**: A quick, targeted LLM call first determines if any such entities are present in the report.
2.  **Extraction**: Only if an entity is detected does the system make a second, focused LLM call to extract its detailed characteristics based on a specific data schema.

This approach significantly improves accuracy and prevents the model from "hallucinating" information that isn't actually in the report.

### 2. Automated Self-Correction Loop
A key feature of this project is its sophisticated, multi-step self-correction mechanism. After an initial extraction, the system uses the LLM to review and refine its own work in a structured, iterative way:
- **Context Scoping**: The system intelligently provides the LLM with either a small text snippet (for high precision on a single entity) or the full report (for broader context).
- **Two-Phase Correction**: For every piece of data, the system runs a two-phase correction:
    1.  **Fill Empty Fields**: It first asks the LLM to identify and fill in any data points that were missed in the initial pass.
    2.  **Correct Incorrect Fields**: It then asks the LLM to review the filled data and correct any inaccuracies it finds.

This automated "peer review" process is built directly into the workflow, ensuring the highest possible data quality and reliability.

### 3. Schema-Driven and Configurable Architecture
The entire extraction process is driven by a set of JSON schemas. This makes the system highly configurable and extensible, allowing it to be easily adapted to new report formats or to extract different types of information without changing the core code.

## Clinical Impact & Significance

By transforming unstructured reports into clean, reliable, and machine-readable data, this project has a significant impact on clinical research and data analysis. It unlocks the valuable information trapped in text-based reports, enabling large-scale studies, the development of predictive models, and a deeper understanding of disease patterns. This toolkit is a critical step towards making clinical data more FAIR (Findable, Accessible, Interoperable, and Reusable).

[Back to all projects](../README.md)