# Architecture Overview

## System Architecture

LingoLive follows a modular architecture designed for extensibility and platform independence.

### Core Components

1. **Audio Processing Layer**
   - Continuous audio capture
   - Echo cancellation
   - Audio stream management
   - Speaker identification preparation

2. **Speech Recognition Layer**
   - Whisper.cpp integration
   - Streaming mode implementation
   - Speaker diarization preparation
   - Language detection

3. **Translation Layer**
   - NLLB-200 integration
   - ONNX Runtime implementation
   - Language pair management
   - Extensible engine architecture

4. **Voice Synthesis Layer**
   - Coqui TTS integration
   - System TTS fallback
   - Speed optimization
   - Speaker-specific voice management

5. **Session Management**
   - Buffer management
   - Language configuration
   - State management
   - Pipeline control

### Data Flow
```mermaid
graph LR
    A[🎤 Microphone] --> B[🧠 Whisper.cpp (STT)]
    B --> C[✏️ Source Text]
    C --> D[🌍 NLLB-200 (Translation)]
    D --> E[🗣️ Translated Text]
    E --> F[🔊 Coqui TTS]
    F --> G[🎧 Audio Output]
```

### Technical Constraints
- Offline-first approach
- < 1s latency target
- Full-duplex operation
- Echo cancellation
- Modular design
- Memory optimization
- Battery efficiency

### Development Guidelines
1. Use Clean Architecture
2. Implement MVI pattern
3. Use Kotlin Coroutines for async operations
4. Follow Material 3 design guidelines
5. Implement comprehensive logging
6. Use ViewBinding for UI
7. Implement proper error handling
8. Add unit tests for core functionality 