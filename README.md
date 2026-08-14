# Minutes

A desktop app that records a meeting, transcribes it on your machine, and writes
the minutes.

One binary. Audio capture, transcription and storage all run locally — the audio
never leaves the machine. Summaries are written by `api.hanzo.ai`; pick a local
model instead if you would rather nothing leave at all.

## How it works

Tauri 2 shell, Next.js interface, Rust core. The core captures the microphone and
system output, transcribes with whisper-rs or Parakeet in-process, stores
everything in SQLite under the app data directory, and sends the transcript to a
summary model to produce markdown minutes.

- Architecture: [docs/architecture.md](docs/architecture.md)
- Build from source: [docs/BUILDING.md](docs/BUILDING.md)
- GPU acceleration: [docs/GPU_ACCELERATION.md](docs/GPU_ACCELERATION.md)
- Privacy: [PRIVACY_POLICY.md](PRIVACY_POLICY.md)

## Summary models

The default is `best` on `api.hanzo.ai` — a routing alias, so the app follows the
catalog rather than pinning a version that ages out. Set your key in Settings; it
is stored in the local database and never committed.

Ollama, OpenAI, Claude, Groq, OpenRouter and any OpenAI-compatible endpoint also
work, along with a fully offline built-in model.

## Build

```
cd frontend
pnpm install
pnpm tauri:build
```

## Credit

Minutes is a fork of [meetily](https://github.com/Zackriya-Solutions/meeting-minutes)
by Zackriya Solutions, used under the MIT licence. Both copyright notices are in
[LICENSE.md](LICENSE.md).

## License

MIT.
