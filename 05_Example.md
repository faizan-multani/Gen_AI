# Example on OpenAI And Gemini :

## Example with OpenAI KEY :
```
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv() # to load environment variable from .env file

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        { "role": "user", "content": "Hey, can you tell me about AI?"}
    ]
)

print(response.choices[0].message.content)

```

## Example with Gemini KEY :
```
# install the package called 'google genai'

from google import genai

client = genai.Client(
    api_key="AIzaSyBjA34ENgeGNplvIqCP-qcH2fuMkqxdO7o"
)

response = client.models.generate_content(
    model="gemini-2.5-flash", contents="Explain how AI works in a few words"
)

print(response.text)
```

## Note :
- If you dont want to buy OpenAI key ,use gemini key in openai code.
- this might help or work.

### you just need gemini key , base url and gemini model.

```
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv() # to load environment variable from .env file

client = OpenAI(
    api_key="AIzaSyBjA34ENgeGNplvIqCP-qcH2fuMkqxdO7o",
    base_url="https://generativelanguage.googleapis.com/v1beta/"
)

response = client.chat.completions.create(
    model="gemini-2.5-flash",
    messages=[
        { "role": "user", "content": "Hey, can you tell me about AI?"}
    ]
)

print(response.choices[0].message.content)
```