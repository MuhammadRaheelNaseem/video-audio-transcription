# video-audio-transcription

This Python script provides a simple yet effective solution for converting video files into text using speech recognition. It leverages the MoviePy library for video editing and the SpeechRecognition library for transcribing speech to text. The script splits the audio from the video into smaller chunks, transcribes each chunk into text using Google's speech recognition service, and then concatenates the text from all chunks.

### Features:
- Extracts audio from video files.
- Splits audio into smaller chunks for better recognition.
- Utilizes Google's speech recognition service for accurate transcription.
- Easy-to-use interface for converting video to text.

### Dependencies:
- [MoviePy](https://github.com/Zulko/moviepy): A Python library for video editing.
- [SpeechRecognition](https://github.com/Uberi/speech_recognition): A Python library for speech recognition.

### Installation:
```bash
pip install moviepy
pip install SpeechRecognition
```

## Usage

```python
from moviepy.editor import VideoFileClip
import speech_recognition as sr
import os

# Function to split audio into chunks
def split_audio(audio, chunk_size=10):  
    # Split audio into chunks of 10 seconds
    duration = audio.duration
    chunks = []
    for i in range(0, int(duration), chunk_size):
        start_time = i
        end_time = min(i + chunk_size, duration)
        chunk = audio.subclip(start_time, end_time)
        chunks.append(chunk)
    return chunks

# Function to recognize text from audio chunks
def recognize_chunks(chunks):
    recognizer = sr.Recognizer()
    recognized_text = ""
    for chunk in chunks:
        temp_wav_file = "temp.wav"
        chunk.write_audiofile(temp_wav_file)
        with sr.AudioFile(temp_wav_file) as source:
            audio_file = recognizer.record(source)
        os.remove(temp_wav_file)
        try:
            text = recognizer.recognize_google(audio_file)
            recognized_text += text + " "
        except Exception as e:
            print("Error:", str(e))
    return recognized_text

# Main function to convert video to text
def video_to_text(video_path):
    clip = VideoFileClip(video_path)
    audio = clip.audio
    chunks = split_audio(audio)
    text = recognize_chunks(chunks)
    return text.strip()

# Example usage
if __name__ == "__main__":
    video_path = "your_video.mp4"  
    # Replace "your_video.mp4" with the path to your actual video file
    text = video_to_text(video_path)
    if text:
        print("Text from the video:")
        print(text)
```

Make sure to replace `"your_video.mp4"` with the path to your actual video file.

### Contributions:
Contributions are welcome! If you have any ideas for improvements or feature requests, feel free to open an issue or submit a pull request.

### License:
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
