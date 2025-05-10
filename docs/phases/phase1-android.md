# Phase 1: Android MVP

## 🎯 Objective
Create a functional Android prototype capable of:
- Continuous listening
- Transcription with Whisper
- Translation with NLLB-200 (local)
- Speaking translations with Coqui TTS
- Operating in 1:1 (bilingual) mode with extensible structure for group mode

## ✅ Implementation Steps

### 1. 📂 Development Environment Setup
#### Required Permissions
- `RECORD_AUDIO` for microphone access
- `READ_EXTERNAL_STORAGE` or `MANAGE_EXTERNAL_STORAGE` if needed

#### Project Structure

1. **Core Infrastructure**
   - Set up Clean Architecture
   - Implement audio recording/playback
   - Create module interfaces

2. **STT Implementation**
   - Integrate Whisper.cpp
   - Implement streaming mode
   - Basic speaker diarization

3. **Translation Engine**
   - Integrate NLLB-200
   - Create translation pipeline
   - Implement language pair management

4. **TTS Implementation**
   - Integrate Coqui TTS
   - Implement system fallback
   - Optimize for speed

5. **UI/UX**
   - Create conversation interface
   - Implement settings
   - Add language selection

## Technical Requirements

- Android SDK 31+
- Kotlin
- Clean Architecture
- MVI pattern
- Material 3
- ViewBinding
- Flow/LiveData 