---
name: Generate expressive TTS audio with Deepdub
description: Authenticate, pick a voice preset, and stream text-to-speech audio from the Deepdub REST API.
api: openapi/deepdub-api-openapi-original.json
operations: [generateTTS, getVoicePrompts, createVoicePrompt]
method: generated
generated: '2026-07-18'
---

# Generate expressive TTS audio with Deepdub

Use the Deepdub REST API (`https://restapi.deepdub.ai/api/v1`) to synthesize speech.

## Auth
Send every request with the `x-api-key` header. Keys start with `dd-`. A free-trial key
(`dd-00000000000000000000000065c9cbfe`, IP rate-limited, US region only) exists for testing.

## Steps
1. **Choose a voice.** Call `getVoicePrompts` (`GET /voice`) to list your private voice prompts, or
   use a published preset id (e.g. `50a537cf-1ec8-4714-b07e-c589ab76be4b` = Promo/Commercials).
   To add your own, `createVoicePrompt` (`POST /voice`) with a base64-encoded sample (max 20 MB).
2. **Generate.** Call `generateTTS` (`POST /tts`) with `text`, `voicePromptId`, `model`
   (`dd-etts-3.0` recommended), and `locale` (e.g. `en-US`). Response is a raw audio stream —
   `mp3` (default), `opus`, or `mulaw`. For `wav`/`s16le` use the WebSocket API (`wss://wsapi.deepdub.ai`).

## Conventions & errors
- Concurrency limits: 5/customer, 3/IP → HTTP `429`. Out of credits → HTTP `402` InsufficientCredits.
- Errors return `{ success:false, message }` (see errors/deepdub-error-codes.yml).
- No idempotency key; TTS is a stateless streaming call.
