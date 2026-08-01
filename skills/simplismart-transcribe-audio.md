---
name: Transcribe or translate audio with Whisper on Simplismart
description: Send an audio file to Simplismart's Whisper V3 (or V2) endpoint for transcription/translation with word-level timestamps and diarization.
api: openapi/simplismart-whisper-v3-openapi.yml
operations: [transcribeAudioV3, transcribeAudioV2]
---

# Transcribe audio with Whisper

Simplismart hosts Whisper for speech-to-text. V3 adds enhanced language support,
word-level timestamps, and speaker diarization.

## Steps

1. **Authenticate** with `Authorization: Bearer <SIMPLISMART_API_KEY>`.
2. **Submit audio** (`transcribeAudioV3`). POST to `/model/infer/whisper` with the
   audio payload and options (transcription vs translation, VAD, diarization).
   For the older model use `transcribeAudioV2` at `/model/v2/infer/whisper`.
3. **Handle the result.** The response returns transcript text plus optional
   timestamps/diarization depending on the options requested.

## Rules

- Errors: `400` invalid parameters, `401` bad/expired key, `500` server error —
  all as a JSON `{ "detail": "..." }` envelope.
- See the Whisper Deployment Guide for VAD and hallucination-reduction tuning:
  https://docs.simplismart.ai/guides/whisper-deployment-guide
