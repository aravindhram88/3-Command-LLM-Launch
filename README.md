# 3-Command-LLM-Launch
Launch a local, browser-based LLM (Gemma 2B/Llama 3) with Ollama and Open WebUI in just 3 terminal commands. Requires Docker.

This repository provides the fastest, most reliable way to get a complete, self-hosted, lightweight Large Language Model (LLM) chat interface running on your local machine. By leveraging Docker and Ollama.

## Prerequisites (Install Docker Desktop)

The entire workflow relies on containerization. You must have Docker Desktop installed, running, and logged in before starting.

Download Docker Desktop: https://docs.docker.com/desktop/

You will also need to open a Command Prompt, PowerShell, or Terminal with sufficient permissions.

## The 3 Commands to Launch

Execute these three commands sequentially in your terminal. They will set up the server, pull the model, and launch the web interface.

Command 1: Start the Ollama Server Container

This downloads the Ollama image, starts the container, and exposes the model service on port 11434.

    docker run -d --gpus all --name ollama -p 11434:11434 -v ollama:/root/.ollama ollama/ollama

GPU Note: If you do not have an NVIDIA GPU, you must remove the --gpus all flag: this will let the LLM use the just CPU of the machine the docker is installed

    docker run -d --name ollama -p 11434:11434 -v ollama:/root/.ollama ollama/ollama

Command 2: Download the Lightweight LLM (Gemma 2B)

This command executes inside the running Ollama container to download the fast, lightweight Gemma 2B model weights.

    docker exec -it ollama ollama pull gemma:2b

Model Customization: Feel free to swap gemma:2b for other lightweight models:

    docker exec -it ollama ollama pull llama3:8b (Balance of power and size)

    docker exec -it ollama ollama pull phi3:mini (Microsoft's small model)

Command 3: Launch the Open Web UI Chat Interface

This starts the web interface and links it to the Ollama server running in the background.

    docker run -d --name open-webui -p 3000:8080 -v open-webui:/app/backend/data -e OLLAMA_BASE_URL=http://host.docker.internal:11434 ghcr.io/open-webui/open-webui:main

Access Your Chat Interface

After the third command finishes, open your web browser and go to:

http://localhost:3000/

(You will be prompted to create a user account on your first visit.)

Cleanup and Stopping the Services

To stop and remove both containers and free up system resources, run these two simple commands:

Stop the running containers

    docker stop open-webui ollama

Remove the containers (Optional: Only if you want a clean restart)

    docker rm open-webui ollama

++++++++++++++++++++++++++++

# Single-Command Launch (Using yaml file)

If you use the provided docker-compose.yml file, you can launch the entire infrastructure with almost a single command.

Setup (One-Time)

Copy the docker-compose.yml file into a new folder.

Navigate to that folder in your terminal.

The 2 Commands

Execute these two commands sequentially.

Command 1: Start the Server and UI Stack

This launches both the Ollama server and the Open WebUI interface in the background.

    docker compose up -d

Command 2: Download the Lightweight LLM (Gemma 2B)

This command executes inside the running Ollama container to download the model weights.

    docker exec -it ollama ollama pull gemma:2b
