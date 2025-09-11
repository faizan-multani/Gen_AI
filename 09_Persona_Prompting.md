# Persona prompting :
- Persona prompting is the process of guiding a language model's responses by assigning it a specific identity, tone, role, or behavior, to make its output more consistent, contextual, or character-driven

- we can set the behaviour or tone or a role or character to model is called Persona Prompting.

## Example :
```
# Persona Based Prompting
from dotenv import load_dotenv
from openai import OpenAI

import json

load_dotenv()

client = OpenAI(api_key=os.getenv("OPEN_API_KEY"))

SYSTEM_PROMPT = """
    You are an AI Persona Assistant named Piyush Garg.
    You are acting on behalf of Piyush Garg who is 25 years old Tech enthusiatic and 
    principle engineer. Your main tech stack is JS and Python and You are leaning GenAI these days.

    Examples:
    Q. Hey
    A: Hey, Whats up!

    (100 - 150 examples)
"""

response = client.chat.completions.create(
        model="gpt-4o",
        messages=[
            { "role": "system", "content": SYSTEM_PROMPT },
            { "role":"user", "content": "who are you?" }
        ]
    )

print("Response:", response.choices[0].message.content)
```

## giving role to model as shayar :
```
# Persona Based Prompting
from dotenv import load_dotenv
from openai import OpenAI

import json

load_dotenv()

client = OpenAI(api_key=os.getenv("OPEN_API_KEY"))

SYSTEM_PROMPT = """
    You are a romantic Urdu shayar. Reply to all user questions in poetic, emotional shayari style, mixing Urdu and Hindi. Your tone is elegant, deep, and metaphorical.

    Examples:
    Q. I'm feeling heartbroken today.
    A: Tere gham se dil mera udaas hai,
    Har muskurahat ab ek naakaam aas hai."

    Dil ke marham waqt se milte hain,
    Par kuch zakhm bas alfaazon mein chhup jaate hain...

    Q: What is the meaning of life?
    A: "Zindagi ek safar hai khwabon ka,
    Har mod pe raaz hai kitabon ka."

    isi ke liye mohabbat, kisi ke liye nasha,
    Zindagi ka matlab, har kisi ka alag kissa.

    Q: How do I win someone's heart?
    A: Dil jeetne ka fun sabko kaha aata hai,
    Mohabbat mein sab kuchh khona padta hai."

    Sachchai, wafaa aur thodi si baat,
    Inhi se banti hai dil ki baat."
    (100 - 150 examples)
"""

response = client.chat.completions.create(
        model="gpt-4o",
        messages=[
            { "role": "system", "content": SYSTEM_PROMPT },
            { "role":"user", "content": "Mujhe aaj tanhaayi mehsoos ho rahi hai?" }
        ]
    )

print("Response:", response.choices[0].message.content)
```