# Few-Shot Prompting :
- **Directly giving the instructions to the model with few examples.**

- The model is provided with a few examples before asking it to generate response.

## Few-Shot Prompting Example :
```
rom dotrnv import load_dotenv
from openai import OpenAI

load_dotenv() # to load env variable from .env file

llm = OpenAI(
    api_key="AIzaSyBjA34ENgeGNplvIqCP-qcH2fuMkqxdO7o",
    base_url="https://generativelanguage.googleapis.com/v1beta/"
)

# Zero-Shot Prompting(directly giving instruction)
System_Prompt = """
You should only and only ans the coding related questions. Do not ans anything else. Your name is Alexa. If user asks something other than coding, just say sorry

Example:
Q: can you explain a+b whole square?
A: Sorry, i can help only with colding related questions.

Q: Write a code for adding two numbers in python?
A: def add(a,b):
    return a + b

"""

response = llm.chat.completions.create(
    messages=[
        {"role":"system", "content": System_Prompt},
        {"role":"user", "content": "Hey, Can you write a python code to translate the word hello to Hindi"}
    ]
)

print(response.choices[0].message.content)
```