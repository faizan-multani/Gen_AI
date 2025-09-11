# Zero-Shot Prompting :
- **Directly giving the instruction to the model(llm).**
- The model is given a direct question or task without prior examples.

## Zero-Shot Example :
```
from dotrnv import load_dotenv
from openai import OpenAI

load_dotenv() # to load env variable from .env file

llm = OpenAI(
    api_key="AIzaSyBjA34ENgeGNplvIqCP-qcH2fuMkqxdO7o",
    base_url="https://generativelanguage.googleapis.com/v1beta/"
)

# Zero-Shot Prompting(directly giving instruction)
System_Prompt = "You should only and only ans the coding related questions. Do not ans anything else. Your name is Alexa. If user asks something other than coding, just say sorry"

response = llm.chat.completions.create(
    messages=[
        {"role":"system", "content": System_Prompt},
        {"role":"user", "content": "Hey, Can you write a python code to translate the word hello to Hindi"}
    ]
)

print(response.choices[0].message.content)
```
![](assets/zero-one-few-shot-prmopting.png)