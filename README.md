# MicioDev course knowledge

A provenance-first index for the MicioDev Laravel 13 + Vue 3 course. The initial dataset records 11 verified playlist entries and cautious metadata/code-derived practices. It does not contain full transcripts.

There is no long-running server to start in this repository. The workflow is local file preparation, optional local transcription, deterministic rendering, and human-reviewed notes.

## Repository layout

- `sources/playlist.json`: exact 11 video IDs, titles, URLs, topics, and evidence status as a plain JSON array.
- `distillations/course-map.md`: initial course map, with evidence boundaries stated explicitly.
- `distillations/practices.md`: practices supported by public playlist metadata and linked FreelanceDesk repository conventions.
- `templates/video-distillation.md`: template for future per-video notes.
- `raw/`, `private/`, `transcripts/`, and `audio/`: local-only locations for source artifacts; all are ignored by Git.

Raw audio, downloaded captions, and transcripts belong under those ignored directories, never in the repository. Future notes must retain provenance, timestamps, confidence, and verification status.

## End-to-end local workflow

### 1. Clone or open both repositories

From a workspace directory, use the canonical repository URLs supplied by your project, then open the course repository alongside the toolkit:

```sh
git clone YOUR_MEDIA_DISTILLATION_TOOLKIT_URL media-distillation-toolkit
git clone YOUR_MICIODEV_KNOWLEDGE_URL miciodev-course-knowledge
cd miciodev-course-knowledge
```

If they are already checked out as sibling directories:

```sh
cd /path/to/workspace/miciodev-course-knowledge
ls ../media-distillation-toolkit
```

PowerShell:

```powershell
git clone YOUR_MEDIA_DISTILLATION_TOOLKIT_URL media-distillation-toolkit
git clone YOUR_MICIODEV_KNOWLEDGE_URL miciodev-course-knowledge
Set-Location miciodev-course-knowledge
Get-ChildItem ..\media-distillation-toolkit
```

### 2. Make the toolkit available

The toolkit has no runtime dependencies. Choose either an editable install or a checkout-only `PYTHONPATH`.

Editable install, POSIX shell:

```sh
python3 -m venv ../media-distillation-toolkit/.venv
. ../media-distillation-toolkit/.venv/bin/activate
python -m pip install -e ../media-distillation-toolkit
```

Editable install, PowerShell:

```powershell
py -m venv ..\media-distillation-toolkit\.venv
. ..\media-distillation-toolkit\.venv\Scripts\Activate.ps1
py -m pip install -e ..\media-distillation-toolkit
```

Without installing, POSIX shell:

```sh
export PYTHONPATH="../media-distillation-toolkit${PYTHONPATH:+:$PYTHONPATH}"
python3 -m media_distill --help
```

Without installing, PowerShell:

```powershell
$toolkit = (Resolve-Path ..\media-distillation-toolkit).Path
$env:PYTHONPATH = "$toolkit;$env:PYTHONPATH"
py -m media_distill --help
```

### 3. Validate all 11 playlist entries

`sources/playlist.json` is intentionally a plain array for source fidelity. It is not directly a toolkit manifest: `media_distill validate` requires an object with `source_url` and `videos[]`. Derive an ignored, toolkit-shaped copy locally, then validate that copy:

POSIX shell:

```sh
mkdir -p private
python3 - <<'PY'
import json
from pathlib import Path

entries = json.loads(Path("sources/playlist.json").read_text(encoding="utf-8"))
assert len(entries) == 11
manifest = {
    "source_url": entries[0]["source"],
    "videos": [
        {"id": entry["id"], "title": entry["title"], "url": entry["url"]}
        for entry in entries
    ],
}
Path("private/playlist-manifest.json").write_text(
    json.dumps(manifest, ensure_ascii=False, indent=2) + "\n",
    encoding="utf-8",
)
PY
python3 -m media_distill validate private/playlist-manifest.json
```

PowerShell:

```powershell
New-Item -ItemType Directory -Force private | Out-Null
@'
import json
from pathlib import Path

entries = json.loads(Path("sources/playlist.json").read_text(encoding="utf-8"))
assert len(entries) == 11
manifest = {
    "source_url": entries[0]["source"],
    "videos": [
        {"id": entry["id"], "title": entry["title"], "url": entry["url"]}
        for entry in entries
    ],
}
Path("private/playlist-manifest.json").write_text(
    json.dumps(manifest, ensure_ascii=False, indent=2) + "\n",
    encoding="utf-8",
)
'@ | py -
py -m media_distill validate private\playlist-manifest.json
```

Do not replace the source array with the derived manifest in the repository; the derived file is local and ignored.

### 4. Keep local media and transcript artifacts ignored

```sh
mkdir -p raw private transcripts audio
```

```powershell
New-Item -ItemType Directory -Force raw, private, transcripts, audio | Out-Null
```

Put locally retrieved audio, captions, intermediate JSON, and rendered transcript Markdown only in those directories. Normalize a selected local source to 16 kHz mono WAV with the [`ffmpeg` recipe in the toolkit ASR quickstart](../media-distillation-toolkit/docs/quickstart-asr.md).

### 5. Transcribe one selected video

Use the documented faster-whisper baseline in [`media-distillation-toolkit/docs/quickstart-asr.md`](../media-distillation-toolkit/docs/quickstart-asr.md), in its separate optional environment. The following compact variant assumes `audio/<video-id>-16k-mono.wav` already exists and writes only ignored output:

```sh
python3 -m pip install faster-whisper  # run only inside the optional ASR environment
python3 - <<'PY'
import json
from pathlib import Path
from faster_whisper import WhisperModel

video_id = "<VIDEO_ID>"
model = WhisperModel("large-v3", device="cuda", compute_type="float16")
segments, info = model.transcribe(
    f"audio/{video_id}-16k-mono.wav",
    language="it",
    vad_filter=True,
)
result = {
    "video_id": video_id,
    "language": info.language,
    "segments": [
        {"start": s.start, "end": s.end, "text": s.text.strip()}
        for s in segments
    ],
}
Path(f"transcripts/{video_id}.json").write_text(
    json.dumps(result, ensure_ascii=False, indent=2) + "\n",
    encoding="utf-8",
)
PY
```

Do not run that installation in the dependency-free toolkit environment if you want to keep it minimal; use the isolated environment described in the toolkit quickstart. Replace `<VIDEO_ID>` with one ID from `sources/playlist.json`. The command assumes the audio was obtained lawfully and is already local; this repository does not download it.

PowerShell transcription variant, after activating the optional environment:

```powershell
@'
import json
from pathlib import Path
from faster_whisper import WhisperModel

video_id = "<VIDEO_ID>"
model = WhisperModel("large-v3", device="cuda", compute_type="float16")
segments, info = model.transcribe(
    f"audio/{video_id}-16k-mono.wav",
    language="it",
    vad_filter=True,
)
result = {
    "video_id": video_id,
    "language": info.language,
    "segments": [
        {"start": s.start, "end": s.end, "text": s.text.strip()}
        for s in segments
    ],
}
Path("transcripts").mkdir(parents=True, exist_ok=True)
Path(f"transcripts/{video_id}.json").write_text(
    json.dumps(result, ensure_ascii=False, indent=2) + "\n",
    encoding="utf-8",
)
'@ | py -
```

### 6. Render transcript Markdown

After the JSON exists, render it without adding inferred text:

```sh
python3 -m media_distill render \
  transcripts/<VIDEO_ID>.json \
  --output transcripts/<VIDEO_ID>.md
```

With an editable install, use `micio-distill` instead:

```sh
micio-distill render transcripts/<VIDEO_ID>.json --output transcripts/<VIDEO_ID>.md
```

### 7. Create a per-video note from the template

Only after checking the transcript and provenance, create a note from the existing template:

```sh
cp templates/video-distillation.md distillations/<VIDEO_ID>.md
```

PowerShell:

```powershell
Copy-Item templates\video-distillation.md distillations\<VIDEO_ID>.md
```

Fill in the exact video metadata, retrieval details, timestamps, confidence, verification status, and source links. Do not invent content from an absent or unchecked transcript.

### 8. Run JSON, toolkit, and repository checks

From the course repository:

```sh
python3 -m json.tool sources/playlist.json >/dev/null
python3 -m media_distill validate private/playlist-manifest.json
python3 -m unittest discover -s ../media-distillation-toolkit/tests -v
git status --short
git diff --check
```

PowerShell:

```powershell
py -m json.tool sources/playlist.json | Out-Null
py -m media_distill validate private\playlist-manifest.json
py -m unittest discover -s ..\media-distillation-toolkit\tests -v
git status --short
git diff --check
```

Inspect the rendered Markdown and `git status` before proposing any repository change. Private artifacts should remain untracked and ignored.

## Evidence boundary

The currently committed material is metadata-derived or code-derived: playlist IDs, titles, URLs, topic grouping, and observations tied to the linked public repositories. It is not transcript-derived. A transcript-derived claim may be added only after a local transcript or caption source is recorded with provenance, timestamps, confidence, and human verification. See [`docs/transcription-workflow.md`](docs/transcription-workflow.md) and the toolkit's [`docs/asr-backends.md`](../media-distillation-toolkit/docs/asr-backends.md).

## Sources

- Playlist: https://www.youtube.com/playlist?list=PLHG2hvLRbYZI
- Official feed: https://www.youtube.com/feeds/videos.xml?playlist_id=PLHG2hvLRbYZI
- Project: https://github.com/micio86dev/freelanceDesk
- API: https://github.com/micio86dev/freelancedesk-api
- Frontend: https://github.com/micio86dev/freelancedesk-web
