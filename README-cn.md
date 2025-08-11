# AI GitHub 周报生成器

此脚本可自动生成一份总结您个人 GitHub 提交历史的周报。它使用 `gh` 命令行工具来获取您最近的提交，并利用 AI 模型（如 DeepSeek、OpenAI 或 OpenRouter）来创建一份简洁、易读的中文报告。

## 特性

*   使用 `gh` 命令行工具从 GitHub 获取您最近的提交。
*   使用可配置的 AI 模型生成技术摘要。
*   支持按名称筛选仓库（包含/排除）。
*   允许自定义报告长度和时间范围（从特定日期开始或最近 N 天）。
*   支持多种 AI 服务（DeepSeek、OpenAI、OpenRouter 以及任何与 OpenAI 兼容的 API）。

## 环境要求

1.  **Python 3.6+**
2.  **GitHub CLI (`gh`)**: 您必须安装并认证 `gh`。
    *   从 [cli.github.com](https://cli.github.com/) 安装。
    *   运行 `gh auth login` 进行认证。
3.  **Python 依赖**:
    ```bash
    pip install openai
    ```

## 安装与配置

1.  **克隆仓库：**
    ```bash
    git clone https://github.com/your-username/AIWeekly.git
    cd AIWeekly
    ```
2.  **安装依赖：**
    创建一个 `requirements.txt` 文件，内容如下：
    ```
    openai
    ```
    然后运行：
    ```bash
    pip install -r requirements.txt
    ```
3.  **配置 API 密钥：**
    您需要为您想使用的 AI 服务提供 API 密钥。这通过环境变量完成。

    **使用 DeepSeek (默认):**
    ```bash
    export DEEPSEEK_API_KEY="your-deepseek-api-key"
    ```

    **使用 OpenAI:**
    ```bash
    export OPENAI_API_KEY="your-openai-api-key"
    export OPENAI_BASE_URL="https://api.openai.com/v1"
    export OPENAI_MODEL="gpt-4"
    ```

    **使用 OpenRouter:**
    ```bash
    export OPENAI_API_KEY="your-openrouter-key"
    export OPENAI_BASE_URL="https://openrouter.ai/api/v1"
    export OPENAI_MODEL="anthropic/claude-3.5-sonnet"
    ```

## 使用方法

首先，赋予脚本执行权限：
```bash
chmod +x ai-weekly.py
```

**基本用法：**
*   生成过去 7 天的报告 (默认, 约 500 字):
    ```bash
    ./ai-weekly.py
    ```
*   生成自定义风格和长度的报告:
    ```bash
    ./ai-weekly.py 简洁的 技术 总结 --chars 200
    ```

**高级用法：**
*   **自定义长度:**
    ```bash
    ./ai-weekly.py --chars 1000
    ```
*   **筛选仓库 (包含):**
    ```bash
    ./ai-weekly.py --filter "project-a|project-b"
    ```
*   **排除仓库:**
    ```bash
    ./ai-weekly.py --exclude "test-repo|archive"
    ```
*   **自定义时间范围:**
    ```bash
    ./ai-weekly.py --days 14  # 最近 14 天
    ./ai-weekly.py --since "2024-01-15" # 从特定日期开始
    ```
*   **显示原始提交:**
    如果只想查看获取的提交而不生成报告:
    ```bash
    ./ai-weekly.py --raw
    ```
*   **覆盖 API 密钥:**
    ```bash
    ./ai-weekly.py --api-key "sk-your-key"
    ```

## 命令行参数

| 参数        | 缩写 | 描述                                                                    | 默认值  |
|-------------|------|-------------------------------------------------------------------------|---------|
| `--filter`  | `-f` | 用于**包含**匹配仓库名称的正则表达式。                                  |         |
| `--exclude` | `-e` | 用于**排除**匹配仓库名称的正则表达式。                                  |         |
| `--raw`     |      | 如果指定，将打印原始提交数据，而不是生成 AI 报告。                        | `false` |
| `--chars`   | `-c` | 生成报告的最大字数。                                                    | `500`   |
| `--api-key` | `-k` | 您的 AI 服务 API 密钥。会覆盖环境变量。                                 |         |
| `--since`   |      | 获取提交的起始日期 (例如, "2024-01-15")。不能与 `--days` 同时使用。     |         |
| `--days`    | `-d` | 包含过去多少天的提交历史。不能与 `--since` 同时使用。                   | `7`     |
| `style`     |      | (位置参数) 对 AI 写作风格的额外要求 (例如, `简洁的 技术 总结`)。        |         |
