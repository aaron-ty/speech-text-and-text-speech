---

# Multimodal Conversational Backend

Speech, Text, and Image Orchestration Service

## Overview

This project is a backend driven multimodal interaction service that supports speech input, text interaction, speech synthesis, and image generation through a unified web interface.

The system is designed as an orchestration layer rather than a single model demo. It coordinates multiple AI capabilities including speech transcription, text generation, speech synthesis, and image generation while handling media processing, request flow, and API boundaries in a production style backend.

The primary goal of this project is to demonstrate how to integrate and operate heterogeneous AI models inside a real web system with clear separation of concerns, extensibility, and operational awareness.

This is not a chatbot toy. It is a backend integration pattern.

## Core Capabilities

1. Speech to text transcription using Whisper with support for multilingual audio input.
2. Text to speech synthesis using Coqui TTS with pluggable voice models.
3. Text driven image generation via local models or Hugging Face hosted APIs.
4. Unified Django backend responsible for request routing, media handling, and model orchestration.
5. Web based interface for interacting with the system through both microphone and text input.

A short screen recording of the system in use is available here
[https://flonnect.com/video/6c72de656e61-4039-a3fa-9b5493bc7b23](https://flonnect.com/video/6c72de656e61-4039-a3fa-9b5493bc7b23)

## System Design

The system follows a backend first architecture.

1. Django acts as the orchestration and integration layer.
2. Audio input is captured, normalized, and passed through a transcription pipeline.
3. Transcribed text flows through response generation and optional image generation.
4. Generated responses are optionally converted back into audio using TTS.
5. Media artifacts are managed explicitly rather than implicitly to avoid tight coupling to any single model provider.

The design favors replaceable components. Whisper and Coqui are default implementations, not hard dependencies. The system is structured so that models can be swapped or API based services can be introduced without rewriting the request flow.

## Why This Exists

Most AI demos collapse under real world constraints because they couple UI logic, model logic, and infrastructure together.

This project intentionally separates those concerns.

It demonstrates how to:

1. Operate AI models behind a web service boundary.
2. Handle audio and image workloads in a backend environment.
3. Design for future replacement of models without breaking the application.
4. Treat AI components as infrastructure, not magic.

## Running the System

The recommended way to run the application is via Docker to ensure consistent handling of audio dependencies such as FFmpeg.

### Docker Based Execution

1. Build and start the services.

   ```
   docker-compose up --build
   ```

2. Access the application.

   ```
   http://localhost:8000/api/interface/
   ```

FFmpeg is required for audio processing. If it is not present in the container, it can be installed directly inside the running container without rebuilding the image.

This mirrors real operational scenarios where hot fixes are sometimes necessary.

### Local Execution

Local execution is supported for development and debugging.

1. Create a virtual environment.
2. Install dependencies from requirements.txt.
3. Apply Django migrations.
4. Start the development server.

The interface is available at the same endpoint as the Docker setup.

## Model Integration

### Speech to Text

Whisper is used for transcription due to its strong multilingual performance and predictable behavior on noisy input.

Both local inference and API based usage are supported. The backend abstracts this choice so the calling code does not change.

### Text to Speech

Coqui TTS is used for speech synthesis. Voice models can be swapped without impacting request handling logic.

### Image Generation

Image generation is handled through an abstraction that supports both local models and Hugging Face hosted endpoints.

API keys are intentionally externalized to avoid hard coupling secrets to the codebase.

## Security and Configuration

Secrets are loaded from an isolated configuration file and are not intended to live in source control long term.

This project demonstrates the pattern, not the final deployment posture.

## What This Project Demonstrates

1. Backend orchestration of multiple AI workloads.
2. Media processing inside a web service.
3. Model agnostic design.
4. Operational awareness beyond simple demos.

## License

MIT License.

---
