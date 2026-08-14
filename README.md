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

## Releasing

There is no release yet, and two things must be settled before there is one.

**The updater's signing key is still upstream's.** `tauri.conf.json` points the
updater at this repo's releases, but the `pubkey` it verifies against is the
minisign key from meetily, whose private half we do not have. Nothing is
exposed by this — the endpoint serves nothing, so no update is offered, and a
manifest we cannot sign is a manifest the app would reject. It is a trap rather
than a hole: the first person to cut a release will produce a binary whose
updater can never succeed. Generate a Hanzo key with `tauri signer generate`,
replace the `pubkey`, and keep the private half in KMS. Do not answer a failing
signature check by turning the check off.

**ffmpeg and the Parakeet models still come from upstream's hosts.**
`src-tauri/build/ffmpeg.rs` downloads five binaries from a Zackriya GitHub
repository at build time, and the Parakeet engine fetches models from
`meetily.towardsgeneralintelligence.com`. Both work and neither was changed,
because inventing a Hanzo URL that does not exist would only have broken the
build. Both are someone else's host in our supply chain, and re-hosting them is
a decision, not a chore.

The Rust side has never been compiled here — this machine has no `cargo`. Run
`cargo check` before trusting any of it.

## Credit

Minutes is a fork of [meetily](https://github.com/Zackriya-Solutions/meeting-minutes)
by Zackriya Solutions, used under the MIT licence. Both copyright notices are in
[LICENSE.md](LICENSE.md).

## License

MIT.
