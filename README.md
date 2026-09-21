# NAMAMI — Multilingual Maritime Voice Agent 🌊🎙️

> A multilingual, speech-to-speech AI agent for maritime and coastal assistance, designed to provide weather warnings, marine forecasts, and practical safety information through natural voice interaction.

## Overview

**NAMAMI** is an agentic AI system that converts spoken user queries into actionable maritime assistance.

The system is designed around a complete **speech → understanding → reasoning → tool use → speech** pipeline.

A user can speak naturally in an Indian language, including code-mixed speech, and the system:

1. Converts speech to text using Whisper.
2. Detects the input language.
3. Translates the query into an internal English representation.
4. Identifies the user's intent and extracts relevant entities/slots.
5. Routes the structured query to an agent.
6. Allows the agent to invoke specialized maritime tools.
7. Generates a response.
8. Converts the response back into the user's original language.
9. Plays the generated response as speech.

The goal is to make AI-based maritime information more accessible to users who may prefer **voice interaction and regional languages over English-first interfaces**.

---

## Architecture

```text
                    ┌─────────────────────┐
                    │   User Speech       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Whisper STT         │
                    │ Speech → Text       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Language Detection  │
                    │ FastText            │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Translation         │
                    │ Regional → English  │
                    └──────────┬──────────┘
                               │
                               ▼
              ┌────────────────────────────────┐
              │     Query Understanding       │
              │                                │
              │ XLM-RoBERTa Intent Classifier │
              │ XLM-RoBERTa NER               │
              └───────────────┬────────────────┘
                              │
                              ▼
                    ┌─────────────────────┐
                    │ LangGraph Agent     │
                    │ + Memory            │
                    └──────────┬──────────┘
                               │
                    ┌──────────┴──────────┐
                    │                     │
                    ▼                     ▼
          ┌─────────────────┐   ┌──────────────────┐
          │ Marine Forecast │   │ Weather Warning │
          │ Tool            │   │ Tool             │
          └────────┬────────┘   └────────┬─────────┘
                   │                     │
                   └──────────┬──────────┘
                              ▼
                    ┌─────────────────────┐
                    │ Agent Response      │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Multilingual TTS    │
                    │ Meta MMS             │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Spoken Response     │
                    └─────────────────────┘
```

---

## Core Capabilities

### 🎙️ Speech Recognition

NAMAMI uses **OpenAI Whisper Medium** for speech-to-text conversion.

The system is designed to work with:

* Hindi
* Tamil
* Telugu
* Bengali
* Malayalam
* Kannada
* Marathi
* Gujarati
* Punjabi
* Odia
* Assamese
* Urdu
* Sanskrit
* Kashmiri
* Sindhi
* Additional regional and low-resource languages supported by the underlying speech-recognition model

The pipeline also considers **code-mixed speech**, such as Hindi-English or Tamil-English conversations.

---

### 🌍 Language Identification

After transcription, FastText language identif
