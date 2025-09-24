# Memory :
- memory refers to the AI’s ability to retain and use information from past interactions or experiences—much like how humans remember things to have more meaningful conversations and make better decisions.

- **It recalls fact and infromation from past experiences.**

## 4 Types of Memory :
## 1. Short-Term Memory (Contextual Memory) :
- **It remember information within the current session or prompt.**

-** Once the session ends/close, memory will be lost/gone.**

- **If we cross the Limits of Token,we lost memory.**

![](assets/SHOT-TERM%20MEMORY.png)

- Basically what happens in short-term memory,we have context window with limit and we cross that limit then memory will lost automatically.

### Example :
- If you say, “**Call me Alex**” and then ask, **“What’s my name?**” during the same session, it will remember and say **“Alex**”, and** if ask after ending of the session it will say i dont know**.

## 2. Long-Term Memory (Persistent Memory) :
- **This memory stores information across sessions.**

- **For example we say my name is faizan khan,it will remember this across the session.**

## 3. Factual Memory :
- **Facts about user.**
- **name,age,contact number,location**

*
## 4. Episodic Memory :
- **The Memory which you want on **demand**.**

### Example :
- i went to paris in 2022 ->**it will be stored in the vector db.**
- **On Demand** = when did i visit to paris?

## Semantic Memory :
- **Memory which is related to Geereal Knowledge.Information.**

### Example :
- **The capital of india is "delhi".**

## **Code/Programm on Memory(Memory Agent) :**
### **Requirements** ->
### **Install mem0** :
```
# bash/
pip install mem0ai
```

### Create **docker-compose.yml file** :
```
services:
  vector-db:
    image: qdrant/qdrant
    ports:
      - 6333:6333
```

### **Make sure your run is running.**

## **Code :** 
```
from dotenv import load_dotenv
from mem0 import Memory
import os
import json

from openai import OpenAI

load_dotenv()

client = OpenAI(api_key=os.getenv("OPEN_API_KEY"))
OPENAI_API_KEY = os.getenv("OPEN_API_KEY")

config = {
    "version": "v1.1",
    "embedder": {
        "provider": "openai",
        "config": { "api_key": OPENAI_API_KEY, "model": "text-embedding-3-small" }
    },
    "llm": {
        "provider": "openai",
        "config": { "api_key": OPENAI_API_KEY, "model": "gpt-4.1" }
    },
    "vector_store": {
        "provider": "qdrant",
        "config": {
            "host": "localhost",
            "port": 6333
        }
    }
}

mem_client = Memory.from_config(config)

while True:
    user_query = input("> ")

    # For Relavent Search ->
    search_memory = mem_client.search(query=user_query, user_id="faizankhan")

    memories = [
        f"ID: {mem.get("id")}\nMemory: {mem.get("memory")}" 
        for mem in search_memory.get("results")
    ]

    # print("Found Memories", memories)

    SYSTEM_PROMPT = f"""
        Here is the context about the user:

        # The json.dumps() function in Python is used to convert a Python object into a JSON-formatted string.
        {json.dumps(memories)}
    """

    response = client.chat.completions.create(
        model="gpt-4.1-mini",
        messages=[
            { "role": "system", "content": SYSTEM_PROMPT },
            { "role": "user", "content": user_query }
        ]
    )

    ai_response = response.choices[0].message.content

    print("AI:", ai_response)

    mem_client.add(
        user_id="faizankhan",
        messages=[
            { "role": "user", "content": user_query },
            { "role": "assistant", "content": ai_response }
        ]
    )

    print("Memory has been saved...")

```