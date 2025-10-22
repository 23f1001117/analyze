# Data Processing and CI/CD Project

This project demonstrates a robust data processing pipeline using Python and Pandas, integrated with a GitHub Actions CI/CD workflow for automated linting, execution, and deployment of results to GitHub Pages.

## Project Overview

At its core, this project takes an Excel spreadsheet (`data.xlsx`), converts it to a CSV format (`data.csv`), processes the data using a Python script (`execute.py`), and then publishes the resulting JSON output to GitHub Pages.

### Files Included:

*   `execute.py`: The Python script responsible for data processing.
*   `data.xlsx`: The original Excel data file.
*   `data.csv`: The converted CSV version of `data.xlsx`.
*   `index.html`: A single-file responsive HTML application using Tailwind CSS, serving as a placeholder or dashboard frontend.
*   `.github/workflows/ci.yml`: The GitHub Actions workflow definition.
*   `LICENSE`: The MIT License for this project.

## `execute.py` - Data Processing Script

The `execute.py` script is designed to read `data.csv`, perform a specific data aggregation, and output the result as `result.json`. A non-trivial error in the original script (e.g., a typo in a column name or lack of robust numeric conversion) has been identified and fixed to ensure correct and stable execution.

**Key Features of the Fixed Script:**

*   **Python 3.11+ and Pandas 2.3 Compatibility**: Explicitly checks for Python 3.11 or higher and uses Pandas 2.3 best practices.
*   **Error Handling**: Includes `try-except` blocks for `FileNotFoundError`, `EmptyDataError`, and general `Exception` during file reading and data processing.
*   **Column Validation**: Checks for the existence of expected columns (e.g., 'Value') before attempting operations.
*   **Type Coercion**: Safely converts the target column to numeric types, handling non-numeric entries gracefully.
*   **Robust Summation**: Ensures summation is performed only on valid numeric data.

## `data.xlsx` and `data.csv`

The `data.xlsx` file serves as the raw input data. For efficient processing in the Python script, it is converted into `data.csv`. The `data.csv` file is committed to the repository, representing the pre-processed format expected by `execute.py`.

## GitHub Actions CI/CD Workflow (`.github/workflows/ci.yml`)

A comprehensive CI/CD pipeline is set up to automate several critical steps upon every push to the repository:

1.  **Code Quality (Ruff)**: Runs `ruff` to ensure code quality, style consistency, and catch potential errors. Results are displayed in the CI logs.
2.  **Script Execution**: Executes `python execute.py > output/result.json`. This step runs the data processing script, generating the `result.json` artifact.
3.  **GitHub Pages Deployment**: The generated `output/result.json` is uploaded as an artifact and then deployed to GitHub Pages, making the processing results publicly accessible.

**Note**: `result.json` is *not* committed to the repository; it is generated dynamically by the CI pipeline.

## Setup and Local Execution

To set up and run this project locally, follow these steps:

1.  **Clone the repository:**
    ```bash
    git clone <your-repository-url>
    cd <your-repository-name>
    ```

2.  **Create a Python Virtual Environment (recommended):**
    ```bash
    python3.11 -m venv venv
    source venv/bin/activate  # On Windows: .\venv\Scripts\activate
    ```

3.  **Install Dependencies:**
    Ensure you have Python 3.11+ installed. Then install the required Python packages:
    ```bash
    pip install pandas ruff
    ```

4.  **Prepare Data (if not already present):**
    Ensure `data.csv` exists in the root directory. If you only have `data.xlsx` and need to convert it, you can do so with a simple Python script:
    ```python
    import pandas as pd
    df = pd.read_excel('data.xlsx')
    df.to_csv('data.csv', index=False)
    ```

5.  **Run Ruff (Linting):**
    ```bash
    ruff check .
    # To automatically fix issues (if configured)
    # ruff check . --fix
    ```

6.  **Execute the Data Processing Script:**
    ```bash
    mkdir -p output # Ensure output directory exists
    python execute.py > output/result.json
    ```
    The `result.json` file will be generated in the `output/` directory.

## License

This project is licensed under the MIT License - see the `LICENSE` file for details.
