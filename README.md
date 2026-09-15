# Offline AI Chatbot (Local LLM via Ollama + Open WebUI)

A self-hosted, offline chatbot running entirely on my local machine — no cloud API, no internet dependency for inference. Built as a hands-on project to learn how local LLM deployment actually works under the hood.

## Why I built this

I don't come from a computer science or technical background. I started this as a personal challenge: could I actually get a large language model running locally on my own machine, entirely offline, without prior dev experience? Everything here — Docker, containers, local inference, performance tuning — was learned from scratch while building this.

## What it does

- Runs a large language model locally using **Ollama** as the inference engine
- Uses **Open WebUI** (served on `localhost:3000`) as the chat interface
- Everything runs inside **Docker**, so the whole stack is containerized and reproducible

## Tech stack

- Docker (containerization)
- Ollama (local model serving)
- Open WebUI (chat interface)
- Windows + WSL2 (host environment)

## What I learned

- How Docker's WSL2 backend on Windows interacts with antivirus/real-time scanning, and how that can silently tank container performance
- The practical difference between CPU and GPU inference for LLMs, and why hardware matters as much as the model itself
- How model quantization (e.g. Q4 variants) trades a small amount of accuracy for a large speed improvement on constrained hardware
- Docker resource allocation (CPU/RAM limits) and how it affects containerized app performance

## Current limitations

This is a work in progress, not a polished product:

- Running CPU-only (no dedicated GPU), so response times are noticeably slow compared to cloud-based chat tools
- Currently testing smaller, quantized models to improve speed on this hardware
- Windows Defender's real-time scanning of the WSL2 filesystem was identified as a performance bottleneck; exclusions help but don't fully close the gap
- Initially, Docker/localhost would only run with Windows Defender/firewall disabled; after adjusting Defender settings (exclusions instead of full disable), it became usable without turning security off each time

## What's next

- Benchmark response times across different model sizes (3B vs 7B, quantized vs full precision)
- Document optimal Docker resource settings for CPU-only setups
- Explore lighter-weight alternatives for low-resource environments

## Setup

No compose file was used for this project — everything was run directly via `docker run` commands. Steps:

1. Install Docker Desktop
2. Pull and run the Ollama image
3. Pull and run the Open WebUI image, connected to Ollama, exposed on port 3000
4. Add Windows Defender exclusions for the WSL2/Docker data folders (avoids needing to disable security software entirely)
5. Access the chat interface at `http://localhost:3000`
