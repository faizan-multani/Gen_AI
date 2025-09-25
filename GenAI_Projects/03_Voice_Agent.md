# Voice Agents Requirements :
## Install SpeechRecognition :
```
pip install SpeechRecognition
```

## Voice Helper :
```
pip install "openai[voice_helpers]"
```

## Voice Agent With TTs(Text To Speech):
```
import asyncio

from dotenv import load_dotenv

import speech_recognition as sr
from openai import OpenAI
from openai.helpers import LocalAudioPlayer

from openai import AsyncOpenAI
import os

load_dotenv()

client = OpenAI(api_key=os.getenv("OPEN_API_KEY"))

# Creates an asynchronous OpenAI client (needed for text-to-speech).
async_client = AsyncOpenAI(api_key=os.getenv("OPEN_API_KEY"))

# Asynchronous Text-to-Speech Function
async def tts(speech: str):
    async with async_client.audio.speech.with_streaming_response.create(
        model="gpt-4o-mini-tts",
        voice="onyx",
        instructions="Always speak in cheerfull manner with full of delight and happy",
        input=speech,
        response_format="pcm",
    )as response:
        await LocalAudioPlayer().play(response) # Plays the streamed audio response using the built-in local audio player.
        

def main():
    r = sr.Recognizer() # speech_recognition library for converting spoken audio to text.

    with sr.Microphone() as source: # Mic Access
        r.adjust_for_ambient_noise(source) # for background noise
        r.pause_threshold = 2 # after speaking it waits for 2 sec

        SYSTEM_PROMPT = f"""
                You're an expert voice agent. You are given the transcript of what
                user has said using voice.
                You need to output as if you are an voice agent and whatever you speak
                will be converted back to audio using AI and played back to user.
            """

        messages = [
            { "role": "system", "content": SYSTEM_PROMPT },
        ]

        while True:

            print("Speak Something...")
            audio = r.listen(source)

            print("Processing Audio... (STT)")

            # Handles errors if speech is unclear
            try:
                stt = r.recognize_google(audio) # Converts the recorded audio to text using Google STT.
                print("You Said:", stt)
            except sr.UnknownValueError:
                print("Could not understand audio. Please try again.")
                continue
            except sr.RequestError as e:
                print(f"Could not request results from Google STT service; {e}")
                continue

            messages.append({ "role": "user", "content": stt })

            response = client.chat.completions.create(
                model="gpt-4.1-mini",
                messages=messages
            )

            print("AI Response", response.choices[0].message.content) # gives output as text
            # Calls the async tts() function to convert the AI response into audio and plays it.
            asyncio.run(tts(speech=response.choices[0].message.content)) # gives output as voice

# Calls the main() function to start the program.
main()

# summary ->
# Listens to your voice,

# Converts it to text,

# Sends it to ChatGPT for a response,

# Converts the response back into cheerful audio using TTS,

# Plays it to you.
```