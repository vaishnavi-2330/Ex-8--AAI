 <H3>ENTER YOUR NAME: VAISHNAVI T</H3>
<H3>ENTER YOUR REGISTER NO. 212225100057</H3>
<H3>EX. NO.8</H3>
<H3>DATE:29/08/2026</H3>
<H1 ALIGN =CENTER>Implementation of Speech Recognition</H1>
<H3>Aim:</H3> 
 To implement the conversion of live speech to text.<BR>
<h3>Algorithm:</h3>
Step 1: Import the speech_recognition library<Br>
Step 2: Initialize the Recognizer<Br>
Step 3: Create an instance of the Recognizer class, which will be used for recognizing speech.<Br>
Step 4: Set the duration for audio capture<Br>
Step 5: Define a variable to specify the duration (in seconds) for which the program will capture audio from the microphone.<Br>
Step 6: Display a message in the console to prompt the user to speak.<Br>
Step 7: Capture audio from the default microphone<Br>
Step 9: Use the default microphone as the audio source.<Br>
Step 10: Record audio for the specified duration using the Recognizer instance.<Br>
Step 11: Perform speech recognition with exceptional handling:<Br>
•	Attempt to recognize speech from the captured audio using the Google Speech Recognition service.<Br>
•	If successful, print the recognized text.<Br>
•	Handle specific exceptions: If the recognition result is unknown or if there is an issue with the request to the Google Speech Recognition service, print corresponding error messages.<Br>
•	A generic exception block captures any other unexpected errors.<Br>
<H3>Program:</H3>

```.py

from google.colab import output
output.enable_custom_widget_manager()

!pip install SpeechRecognition

from IPython.display import Javascript, display
from google.colab import output
from base64 import b64decode
from pydub import AudioSegment
import speech_recognition as sr

# JavaScript to record audio
RECORD = """
async function recordAudio() {
  const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
  const recorder = new MediaRecorder(stream);

  let chunks = [];

  recorder.ondataavailable = e => chunks.push(e.data);

  recorder.start();

  await new Promise(resolve => setTimeout(resolve, 5000));

  recorder.stop();

  await new Promise(resolve => recorder.onstop = resolve);

  const blob = new Blob(chunks);

  const reader = new FileReader();
  reader.readAsDataURL(blob);

  return await new Promise(resolve => {
    reader.onloadend = () => resolve(reader.result);
  });
}
```
```.py
display(Javascript(RECORD))

# Record audio
audio_data = output.eval_js("recordAudio()")

# Decode and save webm file
audio_bytes = b64decode(audio_data.split(',')[1])

with open("recorded_audio.webm", "wb") as f:
    f.write(audio_bytes)

# Convert webm to wav
sound = AudioSegment.from_file("recorded_audio.webm")
sound.export("recorded_audio.wav", format="wav")

# Speech recognition
recognizer = sr.Recognizer()

with sr.AudioFile("recorded_audio.wav") as source:
    audio = recognizer.record(source)

try:
    text = recognizer.recognize_google(audio)
    print("You said:", text)

except sr.UnknownValueError:
    print("Could not understand audio")

except sr.RequestError as e:
    print("Google API error:", e)

except Exception as e:
    print("Error:", e)
    
```


<H3> Output:</H3>
<img width="1083" height="64" alt="597298601-9bb2126c-b5d1-45a4-bf9a-5ef84e78e96b" src="https://github.com/user-attachments/assets/aba0c3b8-4f25-4636-8175-4f82cab5a109" />

<H3> Result:</H3>
 To implement the conversion of live speech to text is successfully executed .
