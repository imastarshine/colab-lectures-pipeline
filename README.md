# colab-lectures-pipeline

A pipeline that converts your lecture audio files into structured notes:  
`Audio File` → `OpenAI/Whisper` → `Transcription` → `Pyannote Speakers` → `AI Notes`

These notebooks are designed for Google Colab and Google Drive. Since the project is open-source, you can easily modify the pipeline to suit your own needs.

## What the pipeline does
1. Sets up folders and an isolated environment for Whisper and pyannote.
2. Checks imports and GPU availability without loading large models.
3. Transcribes audio and creates `Raw/*_raw.txt` and `Transcripts/*.txt`. Already existing files are skipped; where possible, missing versions are restored from existing ones without new inference.
4. If `GENERATE_NOTE=True`, it runs Qwen in a separate process, extracts metadata, creates a Markdown note (in Russian or English, depending on the notebook version), and shuts down the model server.

## About inference
1. For transcribing audio files, the pipeline uses [openai/whisper-large-v3](https://huggingface.co/openai/whisper-large-v3).
2. For speaker diarization, the pipeline uses [pyannote/speaker-diarization-community-1](https://huggingface.co/pyannote/speaker-diarization-community-1).
3. For generating notes, the pipeline uses [Qwen/Qwen3-14B-GGUF](https://huggingface.co/Qwen/Qwen3-14B-GGUF/blob/main/Qwen3-14B-Q4_K_M.gguf) (Q4_K_M quantization).

## Before you start
1. Before running speaker diarization, accept the access terms for [pyannote/speaker-diarization-community-1](https://huggingface.co/pyannote/speaker-diarization-community-1) and add `HF_TOKEN` to Colab Secrets with Notebook access enabled. All settings are located at the beginning of the first cell.
2. Upload all your lecture audio files to the `lectures/` folder in your Google Drive.

## Folders
- `lectures/Raw/` — clean transcription without timestamps and speakers.
- `lectures/Transcripts/` — transcription with optional timestamps and speakers.
- `lectures/Notes/` — generated notes (Russian or English). The nested `Notes/.metadata/` stores validated metadata to avoid re-requesting it if note generation fails.
- `/content/qwen3-14b-gguf/` — Qwen GGUF file (~9 GB) on Colab's local disk. It doesn't take up space on Google Drive, but is deleted after a runtime reset and must be downloaded again.

## Run in Colab
Choose the language version you prefer:

[![Open in Colab INTL](https://img.shields.io/badge/Open_in_Colab-RU-blue?logo=googlecolab)](https://colab.research.google.com/github/imastarshine/colab-lectures-pipeline/blob/main/Lecture_Pipeline_Colab_INTL.ipynb)
