# AI GitHub Weekly Report Generator

This script automatically generates a weekly report summarizing your personal GitHub commit history. It uses the `gh` CLI to fetch your recent commits and an AI model (like DeepSeek, OpenAI, or OpenRouter) to create a concise, human-readable report. The output is in Chinese.

## Features

*   Fetches your recent commits from GitHub using the `gh` CLI.
*   Uses a configurable AI model to generate a technical summary.
*   Supports filtering repositories by name (include/exclude).
*   Allows customizing the report length and time frame (since a specific date or for the last N days).
*   Supports multiple AI providers (DeepSeek, OpenAI, OpenRouter, and any OpenAI-compatible API).

## Prerequisites

1.  **Python 3.6+**
2.  **GitHub CLI (`gh`)**: You must have `gh` installed and authenticated.
    *   Install it from [cli.github.com](https://cli.github.com/).
    *   Authenticate by running: `gh auth login`
3.  **Python Dependencies**:
    ```bash
    pip install openai
    ```

## Setup

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/AIWeekly.git
    cd AIWeekly
    ```
2.  **Install Dependencies:**
    Create a `requirements.txt` file with the following content:
    ```
    openai
    ```
    Then run:
    ```bash
    pip install -r requirements.txt
    ```
3.  **Configure API Key:**
    You need to provide an API key for the AI service you want to use. This is done via environment variables.

    **For DeepSeek (Default):**
    ```bash
    export DEEPSEEK_API_KEY="your-deepseek-api-key"
    ```

    **For OpenAI:**
    ```bash
    export OPENAI_API_KEY="your-openai-api-key"
    export OPENAI_BASE_URL="https://api.openai.com/v1"
    export OPENAI_MODEL="gpt-4"
    ```

    **For OpenRouter:**
    ```bash
    export OPENAI_API_KEY="your-openrouter-key"
    export OPENAI_BASE_URL="https://openrouter.ai/api/v1"
    export OPENAI_MODEL="anthropic/claude-3.5-sonnet"
    ```

## Usage

Make the script executable:
```bash
chmod +x ai-weekly.py
```

**Basic Usage:**
*   Generate a report for the last 7 days (default, ~500 characters):
    ```bash
    ./ai-weekly.py
    ```
*   Generate a report with a custom style and length:
    ```bash
    ./ai-weekly.py 简洁的 技术 总结 --chars 200
    ```

**Advanced Usage:**
*   **Custom Length:**
    ```bash
    ./ai-weekly.py --chars 1000
    ```
*   **Filter Repositories (include):**
    ```bash
    ./ai-weekly.py --filter "project-a|project-b"
    ```
*   **Exclude Repositories:**
    ```bash
    ./ai-weekly.py --exclude "test-repo|archive"
    ```
*   **Custom Time Frame:**
    ```bash
    ./ai-weekly.py --days 14  # Last 14 days
    ./ai-weekly.py --since "2024-01-15" # Since a specific date
    ```
*   **Show Raw Commits:**
    To see the fetched commits without generating a report:
    ```bash
    ./ai-weekly.py --raw
    ```
*   **Override API Key:**
    ```bash
    ./ai-weekly.py --api-key "sk-your-key"
    ```

## Command-Line Arguments

| Argument      | Short | Description                                                                       | Default |
|---------------|-------|-----------------------------------------------------------------------------------|---------|
| `--filter`    | `-f`  | A regex pattern to **include** repositories whose names match.                    |         |
| `--exclude`   | `-e`  | A regex pattern to **exclude** repositories whose names match.                    |         |
| `--raw`       |       | If specified, prints the raw commit data instead of generating an AI report.      | `false` |
| `--chars`     | `-c`  | The maximum character count for the generated report.                             | `500`   |
| `--api-key`   | `-k`  | Your AI service API key. Overrides environment variables.                         |         |
| `--since`     |       | The start date for fetching commits (e.g., "2024-01-15"). Cannot be used with `--days`. |         |
| `--days`      | `-d`  | The number of days of commit history to include. Cannot be used with `--since`.   | `7`     |
| `style`       |       | (Positional) Additional instructions for the AI's writing style (e.g., `简洁的 技术 总结`). |         |
