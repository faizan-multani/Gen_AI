# Multi Model Agent :
### It's an AI systems that can process, understand, and generate multiple types of data modalities, such as:

- Text

- Images

- Audio

- Video

- Code

- 3D data, etc.

## Multi-modal :
- Involving more than one modality.

## Modality :
- A type or format of data (e.g., text, image, audio).

## Agent :
- An AI system that can take actions or perform tasks autonomously

## Example Use Cases :
### Visual Question Answering :

- Input: Image + Question ("What is the person in the image doing?")

- Output: Textual answer ("She is cooking.")

### AI Personal Assistant :

- Input: Voice command + calendar image + email text

- Task: Schedule a meeting and send invites

### Autonomous Agents (like robots) :

- Input: Camera vision (images/video) + audio commands

- Action: Navigate or manipulate objects

### Interactive Chatbots :

- Input: User shares a photo and asks, "Can you describe this?"

- Output: The chatbot replies with a caption or analysis

## **Image-To-Text** Multi Model Agent :
```
from dotenv import load_dotenv
from openai import OpenAI
import os

load_dotenv()

client = OpenAI(api_key=os.getenv("OPEN_API_KEY"))

response = client.chat.completions.create(
    # model which accept input as Image.
    model="gpt-4.1-mini",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "Generate a caption for this image"},
                {"type": "image_url", "image_url": {"url": "https://images.pexels.com/photos/988914/pexels-photo-988914.jpeg"}}
            ]
        }
    ]
)

print("Respinse : ", response.choices[0].message.content)
```

## **Audio-To-Text** Multi Model Agent :
```
from openai import OpenAI
from dotenv import load_dotenv
import os

load_dotenv()

client = OpenAI(api_key=os.getenv("OPEN_API_KEY"))

# Transcribe audio file
with open(r"c:\Users\amiti\Downloads\Calm Down-(Mr-Jat.in).mp3", "rb") as audio_file:
    transcript_text = client.audio.transcriptions.create(
    model="whisper-1",
    file=audio_file,
    response_format="text"
)

print("Lyrics of this song :")
print("🎵 :\n", transcript_text)

print("=============================")

summary_response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "user", "content": f"Summarize the following audio transcript in 30 words  :\n{transcript_text}"}
    ]
)

print("Meaning of this song :")
print("✍ :", summary_response.choices[0].message.content)


```