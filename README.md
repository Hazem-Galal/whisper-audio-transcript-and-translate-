# OpenAI Whisper Audio Processing Notebooks

Production-ready Google Colab notebooks for audio transcription and translation using OpenAI's Whisper API.

**Status:** ✅ Production Ready  
**Last Updated:** February 12, 2026  
**Python:** 3.9+  
**Environment:** Google Colab (Python 3)

---

## 📚 Table of Contents

- [Overview](#overview)
- [Available Notebooks](#available-notebooks)
- [Quick Start](#quick-start)
- [Detailed Setup](#detailed-setup)
- [Features](#features)
- [Usage Guide](#usage-guide)
- [Output Files](#output-files)
- [Supported Audio Formats](#supported-audio-formats)
- [Configuration](#configuration)
- [Error Handling](#error-handling)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)

---

## Overview

This repository contains two production-ready Google Colab notebooks for audio processing using OpenAI's Whisper API:

1. **OpenAI_Whisper_French_Translation.ipynb** — Transcribe any language audio and translate to French
2. **Whisper_German_to_English.ipynb** — Translate German speech directly to English

Both notebooks feature:
- ✓ API-based Whisper (no local installation)
- ✓ Automatic audio chunking for large files
- ✓ Secure API key management via Colab Secrets
- ✓ Comprehensive error handling with exponential backoff
- ✓ Production-grade logging and monitoring
- ✓ Structured JSON metadata output
- ✓ Google Drive integration for file persistence
- ✓ Type hints and docstrings on all functions
- ✓ Modular, reusable code structure

---

## Available Notebooks

### 1. OpenAI_Whisper_French_Translation.ipynb

**Purpose:** Transcribe audio in any language and translate the transcript to professional French.

**Workflow:**
```
Audio (Any Language) → Whisper Transcription → French Translation → Output Files
```

**Models Used:**
- Transcription: `gpt-4o-transcribe` (highest quality)
- Translation: `gpt-4o-mini` (cost-efficient)

**Input:** Audio file (any language)  
**Output:** 
- `transcript_original.txt` — Original language transcript
- `translation_french.txt` — Professional French translation
- `transcript_metadata.json` — Structured metadata

**Key Use Cases:**
- Transcribe presentations in any language
- Generate French transcripts for Francophone audiences
- Create multilingual documentation
- Translate spoken content for media localization

---

### 2. Whisper_German_to_English.ipynb

**Purpose:** Translate German audio directly to English text using Whisper's translate mode.

**Workflow:**
```
German Audio → Whisper Translate → English Text → Output Files
```

**Model Used:**
- Translation: `whisper-1` (Translate endpoint - optimized for speech-to-English)

**Input:** German audio file  
**Output:**
- `translation_english.txt` — English translation
- `transcript_metadata.json` — Structured metadata

**Key Use Cases:**
- Translate German meetings to English
- Convert German podcasts to English text
- Process German business communications
- Create English subtitles from German audio

**Advantages of Whisper Translate Mode:**
- Single API call (faster and cheaper)
- Direct speech-to-English (no intermediate transcript)
- Optimized for translation quality
- Handles multiple speakers naturally

---

## Quick Start

### Step 1: Open in Google Colab

1. Go to https://colab.research.google.com
2. Click "File" → "Open notebook"
3. Choose "Upload" tab
4. Select the notebook file from your computer

**OR** use this direct link pattern (after uploading to GitHub):
```
https://colab.research.google.com/github/[your-repo]/blob/main/[notebook-name].ipynb
```

### Step 2: Add Your OpenAI API Key

1. Click the 🔑 icon in the left sidebar
2. Click "Add new secret"
3. Name: `OPENAI_API_KEY`
4. Value: `sk-...` (paste your OpenAI API key)
5. Click "Add secret" to save

**Get your API key:**
- Visit https://platform.openai.com/account/api-keys
- Create new secret key
- Copy the key (only shown once!)

### Step 3: Run the Notebook

1. Click "Runtime" → "Run all"
2. Wait for cell 1 to complete (dependency installation)
3. When prompted, upload your audio file
4. Monitor the progress logs
5. Download results when complete

### Step 4: Download Results

When the notebook finishes:
- Click download buttons that appear in the output
- Files are saved to your Downloads folder
- Results also available in `/tmp/outputs/` within Colab

---

## Detailed Setup

### Prerequisites

- **Google Account** (for Colab)
- **OpenAI API Key** (see Quick Start Step 2)
- **Audio File** (see Supported Audio Formats section)
- **Internet Connection** (for API calls)

### Environment Variables

The notebooks support multiple ways to provide your API key:

**Method 1: Colab Secrets (Recommended)** ✅
```python
# Automatically retrieved from Colab Secrets
from google.colab import userdata
api_key = userdata.get("OPENAI_API_KEY")
```

**Method 2: Environment Variable**
```python
api_key = os.environ.get("OPENAI_API_KEY")
```

**Method 3: Manual Input (Not Recommended)**
```python
import getpass
api_key = getpass.getpass("Enter your OpenAI API key: ")
```

### System Requirements

**Colab provides:**
- ✓ Python 3.9+
- ✓ 100GB+ disk space
- ✓ GPU/TPU (optional, not needed)
- ✓ Pre-installed libraries: numpy, pandas, etc.

**What the notebooks install:**
- `openai>=1.0.0` — OpenAI SDK
- `pydub>=0.25.1` — Audio chunking
- `librosa>=0.10.0` — Audio analysis
- `ffmpeg` — Audio codec support
- `libsndfile1` — Audio library

---

## Features

### Audio Processing

| Feature | Notebook 1 | Notebook 2 |
|---------|-----------|-----------|
| **Input Language** | Any | German (or detect) |
| **Output Language** | French | English |
| **Max File Size** | 500MB | 500MB |
| **Auto-Chunking** | >20MB | >20MB |
| **Chunk Duration** | 5 min | 5 min |
| **Formats Supported** | 6 formats | 6 formats |

### Security

- ✅ No hardcoded API keys
- ✅ Colab Secrets integration
- ✅ API key validation before processing
- ✅ Error handling for auth failures
- ✅ Timeout protection (60s per request)
- ✅ Rate limit handling with backoff

### Reliability

- ✅ Automatic retry on rate limits (exponential backoff)
- ✅ Connection error handling
- ✅ Graceful failure with clear error messages
- ✅ Status tracking for each pipeline stage
- ✅ Comprehensive logging throughout
- ✅ Pre-flight validation checks

### Optimization

- ✅ Automatic chunking to stay within API limits
- ✅ Sequential chunk processing (no race conditions)
- ✅ Merged output preserves order
- ✅ Minimal dependencies (only what's needed)
- ✅ Efficient file I/O with context managers
- ✅ Temporary file cleanup

---

## Usage Guide

### Notebook 1: French Translation Workflow

```python
# Step-by-step execution:

# [1] Setup & dependencies
#     → Installs openai, pydub, librosa

# [2] Configure API
#     → Retrieves and validates API key

# [3] Upload audio
#     → Select file from your computer

# [4] Validate audio
#     → Checks format, size, duration

# [5] Process
#     → Auto-chunks if needed
#     → Transcribes original language
#     → Translates to French

# [6] Review results
#     → Preview text in notebook
#     → Download files
```

**Configuration (Step 6):**
```python
CHUNK_DURATION_MINUTES = 5      # 5-minute chunks
MAX_FILE_SIZE_MB = 20.0         # Chunk if >20MB
TEMPERATURE = 0.0              # Deterministic output
SOURCE_LANGUAGE_HINT = None    # Auto-detect language
MOUNT_GOOGLE_DRIVE = True      # Backup to Drive
```

### Notebook 2: German-to-English Workflow

```python
# Step-by-step execution:

# [1] Setup & dependencies
#     → Installs openai, pydub, librosa

# [2] Configure API
#     → Retrieves and validates API key

# [3] Upload German audio
#     → Select German audio file

# [4] Validate audio
#     → Checks format, size, duration

# [5] Process
#     → Auto-chunks if needed
#     → Translates German → English directly

# [6] Review results
#     → Preview English translation
#     → Download files
```

**Configuration (Step 6):**
```python
CHUNK_DURATION_MINUTES = 5      # 5-minute chunks
MAX_FILE_SIZE_MB = 20.0         # Chunk if >20MB
TEMPERATURE = 0.0              # Deterministic output
SOURCE_LANGUAGE_HINT = "de"    # German language
MOUNT_GOOGLE_DRIVE = True      # Backup to Drive
```

---

## Output Files

### Notebook 1: French Translation

**File 1: `transcript_original.txt`**
```
[Full transcript in original language]

Example (English input):
"The quick brown fox jumps over the lazy dog. This is a sample sentence."
```

**File 2: `translation_french.txt`**
```
[Professional French translation]

Example output:
"Le rapide renard brun saute par-dessus le chien paresseux. Ceci est une phrase exemple."
```

**File 3: `transcript_metadata.json`**
```json
{
  "filename": "audio.mp3",
  "duration_seconds": 45.32,
  "file_size_mb": 2.15,
  "model_transcription": "gpt-4o-transcribe",
  "model_translation": "gpt-4o-mini",
  "detected_language": "en",
  "chunk_count": 0,
  "processing_time_seconds": 32.5,
  "created_at": "2026-02-12T14:30:00.123456+00:00",
  "transcript_length_chars": 150,
  "translation_length_chars": 165
}
```

### Notebook 2: German-to-English

**File 1: `translation_english.txt`**
```
[English translation of German speech]

Example (German input):
"Der schnelle braune Fuchs springt über den faulen Hund. Dies ist ein Beispielsatz."

Output:
"The quick brown fox jumps over the lazy dog. This is a sample sentence."
```

**File 2: `transcript_metadata.json`**
```json
{
  "filename": "german_audio.mp3",
  "duration_seconds": 45.32,
  "file_size_mb": 2.15,
  "model_used": "whisper-1",
  "detected_language": "de",
  "chunk_count": 0,
  "processing_time_seconds": 28.3,
  "created_at": "2026-02-12T14:30:00.123456+00:00",
  "translation_length_chars": 150,
  "source_language": "German",
  "target_language": "English"
}
```

---

## Supported Audio Formats

All notebooks support the following audio formats:

| Format | Extension | Notes |
|--------|-----------|-------|
| MPEG Audio | `.mp3` | Most common, widely supported |
| WAV | `.wav` | High quality, larger files |
| MPEG-4 Audio | `.m4a` | Apple/iTunes standard |
| FLAC | `.flac` | Lossless compression |
| OGG Vorbis | `.ogg` | Open format, good compression |
| MP4 Video | `.mp4` | Extracts audio automatically |

### Audio Requirements

**Recommended Specifications:**
- **Bitrate:** 128 kbps or higher
- **Sample Rate:** 16 kHz or higher
- **Channels:** Mono or Stereo (both supported)
- **Duration:** Up to 12+ hours (with chunking)
- **File Size:** Up to 500MB

**Maximum API Limits (per chunk):**
- File size: 25MB per request
- Notebooks automatically chunk larger files

### File Size Limits

| File Size | Handling | Processing Time |
|-----------|----------|------------------|
| < 20MB | No chunking | Fast (< 1 min) |
| 20-100MB | Auto-chunked (5-min segments) | Medium (2-5 min) |
| 100-500MB | Auto-chunked (5-min segments) | Slow (5-20 min) |
| > 500MB | Rejected | N/A |

---

## Configuration

### Temperature Parameter

Controls randomness in transcription/translation.

```python
TEMPERATURE = 0.0   # Deterministic (recommended for transcription)
TEMPERATURE = 0.5   # Medium variability
TEMPERATURE = 1.0   # Maximum variability
```

**Recommendation:** Use `0.0` for consistent, accurate transcription.

### Chunk Duration

For long audio files, configure chunk length:

```python
CHUNK_DURATION_MINUTES = 5      # Default: 5-minute chunks
CHUNK_DURATION_MINUTES = 10     # Longer chunks (faster but uses more memory)
CHUNK_DURATION_MINUTES = 3      # Shorter chunks (safer for edge cases)
```

**Recommendation:** 5-10 minutes per chunk for optimal performance.

### Language Hints (Notebook 1 only)

For better accuracy, provide language hints:

```python
SOURCE_LANGUAGE_HINT = None      # Auto-detect (default)
SOURCE_LANGUAGE_HINT = "en"      # English
SOURCE_LANGUAGE_HINT = "fr"      # French
SOURCE_LANGUAGE_HINT = "de"      # German
SOURCE_LANGUAGE_HINT = "es"      # Spanish
```

**Full ISO 639-1 Language Codes:** https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes

### Google Drive Integration

Enable or disable automatic backup to Google Drive:

```python
MOUNT_GOOGLE_DRIVE = True       # Enable Drive backup (default)
MOUNT_GOOGLE_DRIVE = False      # Disable Drive backup
```

**When enabled:**
- Files are saved to `/content/drive/MyDrive/transcription_results/`
- Requires OAuth approval on first run
- Provides persistent storage across sessions

---

## Error Handling

The notebooks include comprehensive error handling for common scenarios:

### 1. API Key Errors

**Error Message:**
```
❌ OpenAI API key not found.
Please set it via:
  1. Colab: Click 🔑 icon → Add OPENAI_API_KEY secret
  2. OR set environment: os.environ['OPENAI_API_KEY'] = 'sk-...'
```

**Solution:**
- Verify API key at https://platform.openai.com/account/api-keys
- Add key to Colab Secrets via 🔑 icon
- Ensure key starts with `sk-`

### 2. Rate Limiting

**Error:** "Rate limit exceeded (429)"

**Automatic Handling:**
```
Rate limited. Waiting 30s before retry...
[Automatic retry after 30 seconds]
```

**Manual Action:** If notebook still fails after retries, wait a few minutes and re-run.

### 3. File Format Errors

**Error:** "Unsupported format. Supported: ('.mp3', '.wav', '.m4a', ...)"

**Solution:**
- Convert file to MP3 using ffmpeg:
  ```bash
  ffmpeg -i audio.wav -q:a 9 -n audio.mp3
  ```
- Re-upload the converted file

### 4. File Size Errors

**Error:** "File too large: 600MB (max 500MB)"

**Solution:**
- Audio files >500MB cannot be processed
- Split using ffmpeg:
  ```bash
  ffmpeg -i large_audio.mp3 -ss 00:00:00 -to 01:00:00 part1.mp3
  ffmpeg -i large_audio.mp3 -ss 01:00:00 -to 02:00:00 part2.mp3
  ```

### 5. Connection Errors

**Error:** "API Connection Error"

**Causes & Solutions:**
- Check internet connection
- Verify OpenAI API is accessible: https://status.openai.com
- Wait a few seconds and retry
- Check notebook logs for detailed error

### 6. Duration Detection Failures

**Warning:** "Could not detect duration (non-critical)"

**Impact:** Non-critical - notebook continues processing  
**Solution:** No action needed; duration will show as 0

---

## Troubleshooting

### Common Issues & Solutions

#### Issue: "No module named 'openai'"

**Cause:** Dependencies not installed  
**Solution:**
1. Run Cell 1 (Install Dependencies)
2. Wait for all messages to complete
3. Continue to Cell 2

#### Issue: API key not working

**Cause:** Invalid or expired key  
**Solution:**
1. Go to https://platform.openai.com/account/api-keys
2. Create a new API key
3. Copy the new key
4. Add to Colab Secrets: 🔑 → Add OPENAI_API_KEY

#### Issue: "Timeout waiting for response"

**Cause:** API request taking too long  
**Solution:**
1. Check your internet connection
2. Try a smaller audio file first
3. Reduce CHUNK_DURATION_MINUTES (e.g., 3 minutes)
4. Wait a few minutes and retry

#### Issue: "Google Drive mount failed"

**Cause:** Optional feature - not critical  
**Solution:**
- Set `MOUNT_GOOGLE_DRIVE = False` in configuration
- Notebook will continue without Drive backup
- Results still saved locally to `/tmp/`

#### Issue: Results not downloading

**Cause:** Browser popup blocker  
**Solution:**
1. Allow popups for colab.research.google.com
2. Look for download notification in browser
3. Files should appear in Downloads folder
4. Alternatively, copy from `/tmp/outputs/` manually

#### Issue: Audio chunking creating too many parts

**Cause:** Very large file with default 5-minute chunks  
**Solution:**
```python
CHUNK_DURATION_MINUTES = 10  # Use 10-minute chunks instead
# or
MAX_FILE_SIZE_MB = 40.0      # Wait longer before chunking
```

#### Issue: Translation quality is poor

**Cause:** May need audio clarification or context  
**Solution (Notebook 1 only):**
```python
SOURCE_LANGUAGE_HINT = "en"  # Provide language hint
# And/or add prompt for context:
# (Currently not exposed in UI, requires code edit)
```

---

## FAQ

### Q: Can I use both notebooks for the same audio?

**A:** Yes! You can:
1. Use Notebook 1 to transcribe and translate to French
2. Use Notebook 2 to translate to English
3. Compare results

### Q: How much does it cost to run these notebooks?

**A:** Depends on audio length and API usage:
- Transcription: ~$0.006 per minute of audio
- Translation: Varies by model, typically $0.001-0.01 per request
- Example: 1-hour audio ≈ $0.40-1.00

See OpenAI pricing: https://openai.com/pricing/

### Q: Can I process multiple files at once?

**A:** Current notebooks process one file per run. To process multiple:
1. Run notebook once per file
2. Or modify the notebook to add a loop (advanced)

### Q: Does the notebook work offline?

**A:** No - requires internet connection for:
- API calls to OpenAI
- Library installation from PyPI
- Google Drive sync (if enabled)

### Q: Can I save results to somewhere other than Google Drive?

**A:** Yes - files are always saved to `/tmp/outputs/` locally  
Download them directly from the Downloads UI

### Q: What happens if my Colab session crashes?

**A:** 
- Colab sessions timeout after 30 minutes of inactivity
- Long-running audio (>2 hours) may exceed session limits
- Solution: Use `MOUNT_GOOGLE_DRIVE = True` to auto-backup

### Q: Can I modify the notebooks?

**A:** Yes! The code is fully documented and modular:
- Edit configuration parameters in Step 6
- Modify function implementations for custom behavior
- Fork/clone for version control

### Q: Do you support other languages besides German and French?

**A:** 
- **Notebook 1:** Can transcribe any language, translate to French
- **Notebook 2:** Only supports German input for English output
- To support other languages, edit the notebooks to use different models

### Q: How do I run these notebooks locally?

**A:** Not recommended for local use, but possible:
1. Install dependencies: `pip install openai pydub librosa`
2. Modify file upload parts (remove Colab-specific code)
3. Set `OPENAI_API_KEY` environment variable
4. Run cells in order

For better local support, use: https://github.com/openai/whisper (local Whisper)

### Q: Are my audio files stored by OpenAI?

**A:** 
- Audio files are uploaded to OpenAI for processing
- Files are deleted after 30 days
- Check OpenAI privacy policy: https://openai.com/privacy
- Set `MOUNT_GOOGLE_DRIVE = True` to keep your own backup

### Q: Can I use a different language for translation?

**A:** 
- **Notebook 1:** Modify Step 8 to translate to different languages (requires editing translate_to_french function)
- **Notebook 2:** Only supports English output (Whisper translate always outputs English)

### Q: What's the difference between the two notebooks?

**A:** See "Available Notebooks" section above for detailed comparison.

**Quick Summary:**
| Aspect | Notebook 1 | Notebook 2 |
|--------|-----------|-----------|
| **Input** | Any language | German |
| **Output** | French | English |
| **Models** | gpt-4o-transcribe + gpt-4o-mini | whisper-1 |
| **Steps** | Transcribe → Translate | Direct translate |
| **Cost** | Higher (2 API calls) | Lower (1 API call) |

---

## Support & Resources

### Documentation

- **OpenAI API Guide:** https://platform.openai.com/docs
- **Whisper API Docs:** https://platform.openai.com/docs/guides/speech-to-text
- **Python SDK:** https://github.com/openai/openai-python

### Getting Help

1. **Check Troubleshooting section** above
2. **Review notebook logs** for detailed error messages
3. **OpenAI Support:** https://help.openai.com
4. **Community:** https://community.openai.com

### Reporting Issues

If you encounter bugs:
1. Collect error logs (copy from notebook output)
2. Note your file details (size, format, language)
3. Reproduce with a simple test file
4. Contact OpenAI support with reproduction steps

---

## License & Attribution

**Created:** February 12, 2026  
**Status:** Production Ready  
**Dependencies:**
- OpenAI SDK (Apache 2.0)
- pydub (MIT)
- librosa (ISC)

---

## Changelog

### Version 1.0 (February 12, 2026)
- ✅ Initial release
- ✅ Notebook 1: French translation pipeline
- ✅ Notebook 2: German-to-English translation
- ✅ Auto-chunking for large files
- ✅ Comprehensive error handling
- ✅ Google Drive integration
- ✅ Production logging and monitoring

---

**Last Updated:** February 12, 2026  
**Maintained By:** AI Development Team  
**Status:** Production Ready ✅
