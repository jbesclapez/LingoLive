# Speech-to-Text Implementation Guide

## Overview
Implementation of real-time speech recognition using Whisper.cpp with preparation for speaker diarization.

## 1. Native Integration

### JNI Setup
```kotlin
// app/src/main/cpp/whisper_jni.cpp
extern "C" {
    JNIEXPORT jlong JNICALL
    Java_com_lingolive_stt_WhisperNative_initModel(
        JNIEnv *env, jobject thiz, jstring model_path) {
        // Initialize Whisper model
    }

    JNIEXPORT jstring JNICALL
    Java_com_lingolive_stt_WhisperNative_transcribe(
        JNIEnv *env, jobject thiz, jbyteArray audio_data) {
        // Transcribe audio data
    }
}
```

### Kotlin Interface
```kotlin
// app/src/main/java/com/lingolive/stt/WhisperNative.kt
class WhisperNative {
    private external fun initModel(modelPath: String): Long
    private external fun transcribe(audioData: ByteArray): String
    
    companion object {
        init {
            System.loadLibrary("whisper")
        }
    }
}
```

## 2. Audio Processing

### Audio Recorder
```kotlin
// app/src/main/java/com/lingolive/stt/AudioRecorder.kt
class AudioRecorder(
    private val sampleRate: Int = 16000,
    private val channelConfig: Int = AudioFormat.CHANNEL_IN_MONO,
    private val audioFormat: Int = AudioFormat.ENCODING_PCM_16BIT
) {
    private var audioRecord: AudioRecord? = null
    private val bufferSize = AudioRecord.getMinBufferSize(
        sampleRate, channelConfig, audioFormat
    )
    
    fun startRecording() {
        audioRecord = AudioRecord(
            MediaRecorder.AudioSource.MIC,
            sampleRate,
            channelConfig,
            audioFormat,
            bufferSize
        )
        audioRecord?.startRecording()
    }
    
    fun readAudioData(): ByteArray {
        val buffer = ByteArray(bufferSize)
        audioRecord?.read(buffer, 0, bufferSize)
        return buffer
    }
}
```

### Streaming Manager
```kotlin
// app/src/main/java/com/lingolive/stt/StreamingManager.kt
class StreamingManager(
    private val whisperNative: WhisperNative,
    private val audioRecorder: AudioRecorder
) {
    private val scope = CoroutineScope(Dispatchers.IO + Job())
    
    fun startStreaming() {
        scope.launch {
            while (isActive) {
                val audioData = audioRecorder.readAudioData()
                val transcription = whisperNative.transcribe(audioData)
                // Process transcription
            }
        }
    }
}
```

## 3. Speaker Diarization Preparation

### Speaker Identification
```kotlin
// app/src/main/java/com/lingolive/stt/SpeakerIdentifier.kt
class SpeakerIdentifier {
    private val speakerProfiles = mutableMapOf<Int, SpeakerProfile>()
    
    data class SpeakerProfile(
        val id: Int,
        val language: String,
        val voiceId: String
    )
    
    fun identifySpeaker(audioData: ByteArray): Int {
        // Basic speaker identification logic
        // To be enhanced for multi-speaker support
        return 1 // Default to speaker 1 for MVP
    }
}
```

## 4. Integration with Main Pipeline

### STT Manager
```kotlin
// app/src/main/java/com/lingolive/stt/STTManager.kt
class STTManager(
    private val context: Context
) {
    private val whisperNative = WhisperNative()
    private val audioRecorder = AudioRecorder()
    private val streamingManager = StreamingManager(whisperNative, audioRecorder)
    private val speakerIdentifier = SpeakerIdentifier()
    
    fun initialize() {
        // Load model
        val modelPath = context.getExternalFilesDir(null)?.absolutePath + "/whisper_model.bin"
        whisperNative.initModel(modelPath)
    }
    
    fun startRecognition() {
        audioRecorder.startRecording()
        streamingManager.startStreaming()
    }
}
```

## 5. Error Handling

### STT Exceptions
```kotlin
// app/src/main/java/com/lingolive/stt/STTExceptions.kt
sealed class STTException : Exception() {
    data class ModelLoadError(override val message: String) : STTException()
    data class TranscriptionError(override val message: String) : STTException()
    data class AudioRecordingError(override val message: String) : STTException()
}
```

## 6. Testing

### Unit Tests
```kotlin
// app/src/test/java/com/lingolive/stt/STTManagerTest.kt
class STTManagerTest {
    @Test
    fun `test transcription accuracy`() {
        // Test implementation
    }
    
    @Test
    fun `test speaker identification`() {
        // Test implementation
    }
}
```

## 7. Performance Optimization

### Buffer Management
```kotlin
// app/src/main/java/com/lingolive/stt/BufferManager.kt
class BufferManager {
    private val buffer = CircularBuffer<ByteArray>(BUFFER_SIZE)
    
    fun addAudioData(data: ByteArray) {
        buffer.add(data)
    }
    
    fun getAudioData(): ByteArray {
        return buffer.get()
    }
}
```

## 8. Configuration

### STT Configuration
```kotlin
// app/src/main/java/com/lingolive/stt/STTConfig.kt
data class STTConfig(
    val modelPath: String,
    val sampleRate: Int = 16000,
    val bufferSize: Int = 4096,
    val language: String = "fr",
    val isStreaming: Boolean = true
)
```

## 9. Usage Example

```kotlin
// Example usage in ViewModel
class ConversationViewModel : ViewModel() {
    private val sttManager = STTManager(application)
    
    fun startConversation() {
        sttManager.initialize()
        sttManager.startRecognition()
    }
}
```

## 10. Dependencies

```gradle
dependencies {
    // Core dependencies
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.1"
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:2.6.1"
    
    // Audio processing
    implementation "com.google.android.exoplayer:exoplayer-core:2.19.1"
    
    // Testing
    testImplementation "junit:junit:4.13.2"
    androidTestImplementation "androidx.test.ext:junit:1.1.5"
}
```

## 11. Next Steps

1. Implement proper error handling and recovery
2. Add logging for debugging
3. Optimize buffer management
4. Prepare for multi-speaker support
5. Add performance monitoring
6. Implement proper cleanup and resource management 