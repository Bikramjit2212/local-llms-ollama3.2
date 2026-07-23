# Local LLMs with Ollama 3.2

Generate production-ready Dockerfiles for any programming language using a locally running Large Language Model (LLM) powered by **Ollama** and **Llama 3.2**.

This project demonstrates how to interact with a local LLM using the Ollama Python SDK to automate Dockerfile generation without relying on cloud-based AI services.

---

## 🚀 Features

- 🤖 Uses **Llama 3.2** running locally via Ollama
- 🐍 Python-based implementation
- 📦 Generates Dockerfiles for different programming languages
- 🏗️ Encourages Docker best practices
- ⚡ Runs completely offline after downloading the model
- 🔒 No API keys required

---

## 🛠️ Tech Stack

- Python 3
- Ollama
- Llama 3.2
- Python Ollama SDK

---

## 📂 Project Structure

```text
local-llms-ollama3.2/
│
├── generate_dockerfile.py
├── requirements.txt
└── README.md
```

---

## 📋 Prerequisites

- Python 3.10+
- Ollama installed

Install Ollama:

https://ollama.com/download

Pull the Llama 3.2 model:

```bash
ollama pull llama3.2
```

Verify installation:

```bash
ollama list
```

Expected output:

```text
NAME
llama3.2:latest
```

---

## 📦 Installation

### Clone the repository

```bash
git clone https://github.com/Bikramjit2212/local-llms-ollama3.2.git
cd local-llms-ollama3.2
```

### Create a virtual environment

```bash
python3 -m venv venv
```

### Activate the virtual environment

Linux/macOS

```bash
source venv/bin/activate
```

Windows

```powershell
venv\Scripts\activate
```

### Install dependencies

```bash
pip install ollama
```

or

```bash
pip install -r requirements.txt
```

---

## ▶️ Run the Project

Start the Ollama server (if it isn't already running):

```bash
ollama serve
```

Run the application:

```bash
python3 generate_dockerfile.py
```

Example:

```text
Enter the programming language:
python
```

---

## 📄 Example Output

```dockerfile

    python3 generate_dockerfile.py 
    Enter the programming language: python

    Generated Dockerfile:

    ```dockerfile
    # Stage 1: Build environment
    FROM mcr.microsoft.com/docker-base:19.03.0-asbury

    # Install dependencies
    RUN apt-get update && \
        apt-get install -y build-essential curl wget git zip && \
        rm -rf /var/lib/apt/lists/*

    # Set working directory
    WORKDIR /app

    # Copy requirements and source code
    COPY requirements.txt .
    RUN pip install --no-cache-dir -r requirements.txt

    COPY . .

    # Stage 2: Application environment
    FROM mcr.microsoft.com/docker-base:19.03.0-asbury

    # Install dependencies (in a separate stage to avoid polluting the base image)
    RUN apt-get update && \
        apt-get install -y build-essential curl wget git zip && \
        rm -rf /var/lib/apt/lists/*

    # Copy application code
    COPY --from=previous:app .

    # Run command
    CMD ["python", "-m", "server"]
    ```

    ```dockerfile
    # Multi-stage, minimal Dockerfile for a Python 3.9 distless image
    FROM mcr.microsoft.com/docker-base:19.03.0-asbury AS build

    # Install dependencies in the build stage
    RUN apt-get update && \
        apt-get install -y build-essential curl wget git zip && \
        rm -rf /var/lib/apt/lists/*

    # Copy requirements and source code
    COPY requirements.txt .
    RUN pip3 install --no-cache-dir -r requirements.txt

    # Copy application code
    COPY . .

    # Stage 2: Application environment
    FROM mcr.microsoft.com/docker-base:19.03.0-asbury AS run

    # Run command
    WORKDIR /app
    CMD ["python", "-m", "server"]
    ```

    ```dockerfile
    # Multi-stage, minimal Dockerfile for a Python 3.9 distless image using Alpine
    FROM mcr.microsoft.com/docker-base:19.03.0-asbury AS build

    # Install dependencies in the build stage
    RUN apk update && \
        apk add --no-cache python3 musl-dev && \
        rm -rf /var/lib/apk/*

    # Copy requirements and source code
    COPY requirements.txt .
    RUN pip3 install --no-cache-dir -r requirements.txt

    # Copy application code
    COPY . .

    # Stage 2: Application environment
    FROM mcr.microsoft.com/docker-base:19.03.0-asbury AS run

    # Run command
    WORKDIR /app
    CMD ["python", "-m", "server"]
    ```

    ```dockerfile
    # Multi-stage, minimal Dockerfile for a Python 3.9 distless image using Alpine and official image
    FROM mcr.microsoft.com/dogfood/python:3.9-slim AS build

    # Install dependencies in the build stage
    RUN pip install --no-cache-dir -r requirements.txt

    # Copy application code
    COPY . .

    # Stage 2: Application environment
    FROM mcr.microsoft.com/dogfood/python:3.9-slim AS run

    # Run command
    WORKDIR /app
    CMD ["python", "-m", "server"]
    ```


```

---

## ⚙️ How It Works

1. User enters a programming language.
2. The prompt is sent to the local Llama 3.2 model.
3. Ollama processes the request locally.
4. The generated Dockerfile is returned and displayed in the terminal.

---

## 📌 Current Prompt

The application instructs the model to generate a Dockerfile with:

- Base image
- Dependency installation
- Working directory
- Source code copy
- Application startup command
- Multi-stage build
- Distroless final image

---


## 👤 Author

**Bikramjit Roy**

Cloud & DevOps Engineer

- GitHub: https://github.com/Bikramjit2212
- LinkedIn: https://www.linkedin.com/in/bikramjitroy/