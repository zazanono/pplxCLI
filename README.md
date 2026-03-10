# pplxCLI

A lightweight, fast Bash CLI tool that brings the power of the Perplexity Sonar API directly to your terminal. 

Built entirely in Bash, this tool streams AI responses in real-time with full markdown colorization, contextual awareness, and automatic citation parsing. Perfect for developers who want quick explanations, debugging help, or coding assistance without leaving the command line.

## Features

- **Real-time Streaming:** Uses Server-Sent Events (SSE) to stream tokens instantly, giving a natural typing effect.
- **Markdown Formatting:** Automatically colorizes bold text, inline code, multiline code blocks, and headers directly in your terminal.
- **Smart Citations:** Automatically detects and prints clickable web sources at the end of the stream if the model searched the internet.
- **Context-Aware Modes:** Easily switch between Coding, Debugging, and Explanation modes.
- **Secure Authentication:** Safely loads your API key from a hidden `.secrets` file.

## Prerequisites

Before installing, ensure you have the following installed on your system:
- `bash` (Default on most Unix systems)
- `curl` (For making HTTP requests)
- `jq` (For parsing JSON safely)

## Installation

**1. Clone the repository**
```bash
git clone https://github.com/yourusername/pplxCLI.git
cd pplxCLI
```

**2. Set up your API Key**
Create a hidden secrets file in your home directory to securely store your Perplexity API key:
```bash
echo 'export PPLX_KEY="your-api-key-here"' >> ~/.secrets
```

**3. Install globally (Optional but recommended)**
To use the `pplx` command from any directory, move the script to your local bin folder and add it to your PATH:
```bash
mkdir -p ~/.local/bin
cp pplx ~/.local/bin/pplx
chmod +x ~/.local/bin/pplx

# Add this line to your ~/.zshrc or ~/.bashrc if it isn't there already:
# export PATH="$HOME/.local/bin:$PATH"
```

## Usage

The CLI is designed to take three positional arguments: a mode flag, the context (language/tool), and your query. You can also optionally paste current code to be evaluated.

### Syntax
```bash
pplx [MODE] "Context/Language" "Your query" ["Optional code snippet"]
```

### Available Modes
- `-c` : **Coding Mode** (For generating new code)
- `-d` : **Debugging Mode** (For finding bugs in existing code)
- `-e` : **Explain Mode** (For understanding concepts or syntax)

### Examples

**Ask a simple question:**
```bash
pplx -e "Bash" "How do I extract text from a JSON stream using jq?"
```

**Debug a block of code:**
```bash
pplx -d "Python" "Why is this loop throwing an index out of bounds error?" "for i in range(len(arr)+1): print(arr[i])"
```

## How it works

This script leverages advanced Bash techniques to provide a seamless UX:
- Uses `jq --arg` to safely encode user input into JSON without breaking if quotes or special characters are used.
- Pipes `curl` `--no-buffer` output into a `while read` loop to process Server-Sent Events line-by-line.
- Uses `sed -u` with ANSI escape codes to parse and colorize Markdown syntax on the fly without breaking the continuous stream.
- Safely manages subshells using curly brace grouping `{ }` to ensure citation arrays persist and print accurately after the stream finishes.

## License
MIT License
