# Transcription provenance checklist

Use this checklist for every transcript-backed note. Keep audio, captions, transcript JSON, and rendered Markdown in ignored local directories. The toolkit only validates manifests and renders supplied JSON; it does not retrieve media, download models, or call providers.

## Source and retrieval

- [ ] Source URL is recorded exactly.
- [ ] Video ID is recorded and matches `sources/playlist.json`.
- [ ] Retrieval date and timezone are recorded.
- [ ] Audio or caption input is stored locally under an ignored path.
- [ ] A cryptographic hash of the input audio/caption file is recorded.

## Transcription details

- [ ] Backend name and exact model/checkpoint are recorded.
- [ ] Backend, model, and runtime versions are recorded.
- [ ] Language setting is recorded, including whether Italian was forced or detected.
- [ ] Segment timestamps are retained; word timestamps are identified separately when present.
- [ ] Diarization status is explicit: not run, attempted, or completed.
- [ ] If diarization ran, its model, token/access terms, and speaker-label limitations are recorded.
- [ ] Prompt or decoding configuration version is recorded, or `not applicable` is stated for a plain ASR run.
- [ ] Confidence is marked `low`, `medium`, or `high` with a short reason.
- [ ] A human has checked names, technical terms, timestamps, and important claims against the source.

## Rights and privacy

- [ ] Retrieval and transcription are permitted under the source, project, and applicable privacy rules.
- [ ] No API token, private URL, private media, or personal data is committed.
- [ ] Model, alignment, and diarization terms were checked for the intended use.
- [ ] Sensitive recordings and derived artifacts remain in ignored `raw/`, `private/`, `audio/`, or `transcripts/` paths.

For backend selection and the distinction between ASR timestamps and speaker labels, see the toolkit's [ASR backend comparison](../../media-distillation-toolkit/docs/asr-backends.md) and [optional ASR quickstart](../../media-distillation-toolkit/docs/quickstart-asr.md).
