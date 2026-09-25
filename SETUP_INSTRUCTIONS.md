# Setup instructions

This guide prepares your computer for the course workflows. It installs:

1. Docker, to run Langflow in a reproducible container
2. Ollama, to run a local language model
3. OpenCode, the terminal-based coding assistant used to inspect and adapt projects
4. Langflow  instance, and its persistent data volume.

The commands below are written for macOS or Linux. On Windows, use Docker Desktop and run them in WSL2. **Do never upload API keys, passwords, or personal access tokens to the course repository.**

## 1. Download the course repository

Install Git if it is not already available, then clone the course repository and enter it:

```bash
git clone https://github.com/JonasRigo/AI_for_physics_laboratory.git
cd AI_for_physics_laboratory.
```

## 2. Install and test Docker

Install [Docker Desktop](https://docs.docker.com/get-docker/) and start it. Verify that the Docker command works:

```bash
docker --version
docker run --rm hello-world
```

The second command downloads a small test image and should end with a success message. If it fails, fix Docker before continuing.

## 3. Install Ollama

Install Ollama from the [official download page](https://ollama.com/download). Verify the installation:

```bash
ollama --version
```

Ollama normally runs as a background application after installation. If it is not running, start it with:

```bash
ollama serve
```

Keep that terminal open if your operating system does not start Ollama automatically.

## 4. Install OpenCode through Ollama

OpenCode is the terminal assistant used in the course to inspect files, understand workflows, edit code, and run local checks. The simplest current setup is provided by Ollama:

```bash
ollama launch opencode
```

Follow the interactive prompts to select an available model. This command configures and launches OpenCode using Ollama. It requires a recent Ollama release; update Ollama if the `launch` subcommand is unavailable.

To configure without immediately starting a session, use:

```bash
ollama launch opencode --config
```

Then test OpenCode from the course repository:

```bash
opencode
```

Ask it a harmless question such as “List the files in this repository and identify the README.” OpenCode should be able to read the project directory. Approve file edits and command execution only when you understand what they will do.

### Model and hardware notes

#### Recommended starter model

For a common low-resource starting point, use IBM Granite 3.3 2B:

```bash
ollama pull granite3.3:2b
ollama launch opencode --model granite3.3:2b
```

The `granite3.3:2b` model is relatively small (about 1.5 GB in the Ollama registry) while providing a 128k-token context window, so it is a practical first model for reading course repositories and trying small coding tasks. It is a starter recommendation, not a guarantee that every workflow will perform well. For more capable hardware, try the 8B variant:

```bash
ollama pull granite3.3:8b
ollama launch opencode --model granite3.3:8b
```

If `ollama launch opencode --model ...` is not accepted by your installed Ollama version, run `ollama launch opencode` and select the downloaded model interactively.

Local language models use substantial disk space, memory, and sometimes GPU memory. OpenCode works best with a model and context window large enough for the project: a 64k-token context is recommended. If your computer cannot run a sufficiently large local model, use the smallest model that works for the exercise or use one of the free models provided by OpenCode. Hosted providers may require an API key and may incur cost but, do not hand out your credit card lightly.

OpenCode discovers models from Ollama. Inside OpenCode, use `/models` to inspect and select an available model. Do not paste an API key into `opencode.json`, a flow export, or a Git commit.

Useful references:

- [Ollama installation](https://ollama.com/download)
- [Ollama and OpenCode integration](https://docs.ollama.com/integrations/opencode)
- [OpenCode providers and local Ollama models](https://opencode.ai/docs/providers)

## 5. Start Langflow with Docker

Create a persistent Docker volume. This keeps Langflow's local database and imported flows when the container is stopped or recreated:

```bash
docker volume create langflow-data
```

Start Langflow in the background:

```bash
docker run -d \
  --name langflow \
  -p 7860:7860 \
  -e LANGFLOW_AUTO_LOGIN=true \
  -e LANGFLOW_CONFIG_DIR=/app/langflow \
  -e LANGFLOW_SAVE_DB_IN_CONFIG_DIR=true \
  -e LANGFLOW_SSRF_ALLOWED_HOSTS=host.docker.internal \
  -v langflow-data:/app/langflow \
  langflowai/langflow:latest
```

Open Langflow at:

```text
http://localhost:7860/
```

Use `http`, not `https`. If the page does not open, check the container:

```bash
docker ps
docker logs --tail 100 langflow
```

The first startup may take a little while while Docker downloads the image.

## 6. Install extra Python packages when a flow requires them

Some exported flows use packages that are not in the minimal Langflow image. Install them inside the running container only when the flow reports that they are missing. For example:

```bash
docker exec langflow \
  /app/.venv/bin/python -m pip install trustcall sympy
```

Restart Langflow after installing packages:

```bash
docker restart langflow
```

This installation belongs to the current container. If the container is deleted and recreated, repeat the installation or ask the instructor for the course's pinned image. Do not assume that installing a package on the host computer makes it available inside Docker.

## 7. Import and run a course workflow

1. Open `http://localhost:7860/`.
2. Use **Import** or **Load** to select a workflow JSON file, for example:
   `graph-extraction-review/workflow/Graph Extraction.json`.
3. Inspect the input, model, and output nodes.
4. Configure the model provider. For a fully local setup, select an Ollama model if the flow supports it and use the host address shown by the provider configuration. From inside the container, the host is usually:

   ```text
   http://host.docker.internal:11434
   ```

5. Run the example using the input and instructions in that workflow's README.
6. Save the graph JSON and final response in the workflow's `evidence/` directory.

For command-line execution, the `lfx` command may be available in an existing Python environment. First check:

```bash
lfx --help
```

If it is unavailable, use the Langflow UI unless the instructor has supplied a pinned `lfx` installation. CLI behavior and required extensions can differ between Langflow releases.

## 8. Stop, resume, and remove Langflow

Stop the container without deleting its data:

```bash
docker stop langflow
```

Resume it later:

```bash
docker start langflow
```

Remove only the container while keeping the persistent volume:

```bash
docker rm -f langflow
```

The `langflow-data` volume is separate and is not removed by that command. Do not delete the volume unless you intentionally want to erase the local Langflow database and saved flows:

```bash
docker volume rm langflow-data
```

## 9. Troubleshooting checklist

### `docker: command not found`

Install Docker Desktop, start it, open a new terminal, and repeat `docker --version`.

### Langflow page does not open

Check that Docker is running and that the container is up:

```bash
docker ps
docker logs --tail 100 langflow
```

Use `http://localhost:7860/`, and check that another application is not already using port 7860.

### Ollama connection fails from Langflow

Confirm that Ollama is running and that a model is installed:

```bash
ollama list
curl http://localhost:11434/api/tags
```

From a Docker container on macOS or Windows, use `http://host.docker.internal:11434`, not `http://localhost:11434`. On Linux, `host.docker.internal` may need additional Docker configuration; ask the instructor if that address is unavailable.

### OpenCode cannot find a model

Run `ollama list`, pull or select a completion model, restart OpenCode, and use `/models`. Embedding-only models cannot be used as chat or coding models.

### A flow reports a missing Python package

Read the exact package name in the error and install it inside the Langflow container with the command in Section 6. If the flow uses a custom course extension, use the installation instructions shipped with that workflow.

### A flow asks for an API key

The flow is configured for a hosted provider. Either switch it to a working local Ollama provider or configure the approved hosted provider without committing the key. Never put credentials in a JSON export or repository file.

## 10. Ready-to-submit check

Before beginning an assignment, verify:

```bash
docker ps
ollama list
opencode --help
```

Then confirm that:

- Langflow opens at `http://localhost:7860/`;
- OpenCode starts in the course repository;
- at least one Ollama completion model is available;
- the selected workflow imports without missing-component errors; and
- a small test run produces a saved output.

If any item fails, record the error and the attempted fix in the assignment's evidence or README rather than silently changing the workflow.
