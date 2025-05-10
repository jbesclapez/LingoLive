# LingoLive

Real-time conversation translation app supporting multiple platforms.

## 🚀 Project Overview

LingoLive is a mobile application that enables real-time translation of conversations between two people speaking different languages. The app provides:

- Continuous audio listening
- Automatic speaker identification
- Immediate translation to the interlocutor's language
- Vocal synthesis of translations without perceptible pauses
- Support for Android and iOS, with extensions to smartwatches (Wear OS and Apple Watch)

## 🚀 Key Features

### 🔊 Audio Input
- Continuous listening without button press
- Automatic active speaker identification (speaker diarization)
- Pre-defined language per speaker (e.g., Speaker 1 = Dutch, Speaker 2 = French)

### 🧠 Processing
- Accurate speech transcription
- Dynamic translation of detected phrases
- Full-duplex operation (speaking and listening simultaneously)

### 🗣️ Voice Synthesis
- Vocal reading of translations in target language
- Fluid, natural, and fast voice synthesis
- Echo cancellation to prevent microphone feedback

## 🛠 Technical Stack

### Speech-to-Text (STT)
- Whisper.cpp (local/offline) or Whisper API
- Support for Dutch and French
- Real-time streaming mode
- Automatic speaker diarization

### Translation
- Local translation using NLLB-200 distilled (Meta)
- Extensible to other models: MarianMT, Mixtral, Nous-Hermes
- Modular architecture supporting multiple engines

### Text-to-Speech (TTS)
- Natural voices via:
  - Coqui TTS (local)
  - System fallback (Google TTS, Apple AVSpeechSynthesizer)
- Adjustable speed (speakingRate > 1.2)

## 📱 Platform Support

### Phase 1: Android (Smartphone)
- MVP with STT + Translation + TTS

### Phase 2: Android Watch (Wear OS)
- Microphone interaction + Mobile playback

### Phase 3: iPhone (iOS)
- Android app port with local processing

### Phase 4: Apple Watch
- Companion app with voice interaction and playback

## 🏗 Architecture

### Core Modules
- SpeechRecognizer → Whisper (cpp or API)
- TranslatorEngine → NLLB-200 (ONNX), MarianMT, GPT, etc.
- SpeakerIdentifier → Automatic speaker diarization
- TTSPlayer → Coqui, Android/iOS system
- SessionManager → Buffer, exchange, and language configuration management

### Technical Constraints
- Priority on local processing (offline), with cloud fallback
- Low latency (< 1s between phrase end and vocal translation)
- Simultaneous microphone and speaker compatibility
- Echo cancellation for audio output
- Modular component-based architecture

## 🚀 Getting Started

[Development setup instructions will be added]

## 📚 Documentation

Detailed documentation can be found in the `docs/` directory:
- Architecture overview
- Module specifications
- Implementation guides
- API documentation

## 📄 License

[License information to be added]
