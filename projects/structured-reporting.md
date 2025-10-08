# German Radiology Report Analyzer

## Project Overview

This project is a sophisticated toolkit for analyzing and structuring German-language radiology reports. It leverages the power of Large Language Models (LLMs) to transform complex, unstructured medical texts into a standardized, validated, and machine-readable format. The primary goal is to automate the extraction of key information from these reports, making the data more accessible for clinical research, data analysis, and other applications.

The system is designed to be robust and scalable, with a multi-stage pipeline that includes translation, granular data extraction, and quality control checks.

## Features

-   **Excel-based Batch Processing**: Process multiple reports from a single Excel file.
-   **Automated Translation**: Translates German reports to English to work with a wider range of LLMs.
-   **Granular Data Extraction**: A sophisticated, multi-step extraction process to identify and structure complex medical entities like nodules, lesions, and metastases.
-   **Simple Section Extraction**: Extracts key-value data from standard report sections (e.g., Clinical Indication, Medication History).
-   **Automated Quality Control**: An LLM-based check to identify discrepancies between the original and the structured report.
-   **Detailed Logging**: Generates comprehensive logs for each processed report, detailing every step of the extraction process.
-   **Formatted Reporting**: Produces a clean, human-readable markdown report from the extracted structured data.
-   **Resilient and Scalable**: Built with multiprocessing and retry mechanisms to handle large volumes of reports and unreliable API connections.

## Recent Developments

This project has undergone a significant refactoring to improve the quality and robustness of the extraction pipeline. Key improvements include:

### Advanced Granular Extraction

The extraction process for complex, repeating entities (like nodules, lymph nodes, and PET/CT entities) has been re-architected into a two-step process:

1.  **Detection:** The system first makes a targeted LLM call to determine if any such entities are present in the relevant section of the report. This is a quick, efficient check that avoids unnecessary processing.
2.  **Extraction:** If entities are detected, the system then iterates through each one, making a separate, focused LLM call to extract its detailed characteristics based on a specific JSON schema.

This "detect then extract" approach significantly improves accuracy and reduces the risk of the LLM hallucinating information when an entity is not present.

### Schema-Driven and Configurable

The entire extraction process is driven by a set of JSON schemas located in the `radextract/prompts/fine_grained/` directory. This makes the system highly configurable and extensible:

-   **`GRANULAR_ENTITIES`**: This dictionary in `granular_extraction.py` defines which entities to look for and which detection and extraction schemas to use for each one.
-   **`SIMPLE_SECTIONS`**: This dictionary defines the standard, non-repeating sections of the report to be extracted.

### Intelligent Report Generation

The final report generation is now more intelligent. The `report_generator.py` and `report_formatters.py` scripts work together to create a clean, concise final report. The system now checks if a field has a valid value before including it in the output, so `null`, `None`, or empty strings are automatically excluded.

### Robust and Centralized LLM Calls

All interactions with the LLM have been centralized into a single `call_llm` function in `granular_extraction.py`. This function includes:

-   **Automatic Retries:** Uses the `tenacity` library to automatically retry failed API calls with exponential backoff.
-   **Rate Limit Handling:** Includes a `time.sleep()` to prevent hitting API rate limits.

### Codebase Cleanup

The codebase has been significantly cleaned up:

-   **Legacy Code Removal:** Old, unused scripts like `app.py`, `full_report_pipeline.py`, and `level3_extractor.py` have been removed.
-   **New Directory Structure:** Helper scripts have been moved to a dedicated `scripts/` directory.

## Automated Data Correction

A key feature of this project is its sophisticated, multi-step self-correction mechanism, designed to enhance the accuracy of the extracted data. This process, located in `granular_extraction.py`, doesn't just perform a one-time extraction; it asks the LLM to review and refine its own work in a structured, iterative way.

The upcoming version will enhance this process further by introducing a high-level check to guide the correction process, making it even more robust.

### The Self-Correction Loop

The correction process is triggered immediately after the initial data extraction for every piece of information—from a single nodule to a full report section.

#### Context Scoping

The system intelligently provides different levels of context for correction:
-   **Granular Entities (e.g., a Nodule):** To ensure high precision, the correction context is limited to the specific text snippet describing only that single entity. This prevents the LLM from getting confused by other parts of the report.
-   **Simple Sections (e.g., Clinical Indication):** For broader sections, the context is the entire report text, as relevant information may be scattered throughout the document.

#### Two-Phase Correction

The correction itself is performed in two distinct phases for every extraction:

1.  **Phase 1: Fill Empty Fields**
    -   **Identify Empties:** The system first finds all fields in the extracted data that are empty (`null` or `""`).
    -   **Ask "Which Fields Can Be Filled?":** It then makes an LLM call, providing the context text and the list of empty fields, and asks which ones can be filled based on the text.
    -   **Fill The Fields:** For each field the LLM identifies as "fillable," a second, targeted LLM call is made to ask for the specific value, which is then added to the data.

2.  **Phase 2: Correct Incorrect Fields**
    -   **Identify Inaccuracies:** The system takes the updated data and asks the LLM to identify any fields that contain incorrect information when compared to the source text.
    -   **Fix The Fields:** For each field the LLM flags as incorrect, another targeted call is made, providing the current (wrong) value and asking for the correct one. The data is then updated.

This entire "extract, then correct" cycle makes the pipeline highly reliable, as it has an automated "peer review" process built directly into its workflow.

## Architecture and Core Components

The project is built around a modular architecture, with each component responsible for a specific part of the processing pipeline.

-   **`process_reports.py`**: The main entry point for batch processing. It orchestrates the entire workflow, from reading the input Excel file to writing the final results. It also contains the logic for translation and the final quality control check.
-   **`granular_extraction.py`**: The core of the extraction logic. It contains the multi-stage process for finding and structuring both granular entities and simple sections from the report text. It also contains the centralized `call_llm` function.
-   **`report_generator.py`**: Generates the final, human-readable markdown report from the structured data.
-   **`report_formatters.py`**: Contains functions to format the extracted data for the final report, ensuring that empty fields are not included.
-   **`report_templates.py`**: Contains the templates for the final report.

## Prerequisites

-   Python 3.10 or higher
-   `pip` for package management

## Installation

1.  **Clone the repository:**
    ```bash
    git clone <repository_url>
    cd <repository_name>
    ```

2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

## Configuration

The script requires API credentials to interact with the LLM. These are managed using a `.env` file in the root of the project.

1.  **Create a `.env` file:**
    ```bash
    touch .env
    ```

2.  **Add your credentials to the `.env` file:**
    ```
    API_KEY="your_api_key"
    API_BASE_URL="your_api_base_url"
    MODEL_NAME="your_model_name"
    ```
    Replace the placeholder values with your actual credentials.

## Usage

The primary way to use this project is through the `process_reports.py` script.

**Command:**
```bash
python process_reports.py -i <input_excel_path> -o <output_excel_path>
```

**Arguments:**
-   `-i`, `--input`: The path to the input Excel file containing the reports. Defaults to `test_report.xlsx`.
-   `-o`, `--output`: The path to the output Excel file where the results will be saved. Defaults to `data/output_reports.xlsx`.

**Example:**
```bash
python process_reports.py -i data/unstructured_reports.xlsx -o data/processed_reports.xlsx
```

## Input and Output

### Input Excel File

The input file must be an Excel spreadsheet (`.xlsx`). The script will read all sheets and all columns, and any cell containing a string longer than 30 characters will be treated as a report to be processed. For best results, it is recommended to have a single column named `report` containing the unstructured medical reports.

### Output Excel File

The script will generate a new Excel file with the following columns:

-   **`Original Report`**: The original, unprocessed German report.
-   **`Translated Report`**: The English translation of the report.
-   **`Structured Report`**: The final, human-readable markdown report generated from the extracted data.
-   **`Log`**: A detailed, multi-stage log of the entire extraction process for the report.
-   **`LLM Check`**: The result of the final quality control check, highlighting any potential discrepancies.

The script saves its progress after processing each report, so if it is interrupted, you can resume it without losing your work. Already processed reports will be skipped on subsequent runs.

[Back to all projects](../README.md)