# Chain Of Thought(COT) Prompting :
- It is a prompting method where you **guide the model to generate a step-by-step** reasoning process before arriving at the final answer.

- Instead of just asking for an **answer directly**, you encourage the model to **"think through" the problem**.

## Example : Math Problem 
### Que : A train travels 60 miles in 1 hour and 120 miles in 2 hours. What is the average speed?
### Ans : 
- step 1: **Start🔥**- Let's think step by step- 
- step 2: **Thinking🧠**- The total distance is 60 + 120 = 180 miles. 
- step 3: **Thinking🧠**- The total time is 1 + 2 = 3 hours. 
- step 4: **Thinking🧠**- So the average speed is 180 ÷ 3 = 60 miles per hour.
- step 5: **OUTPUT 🤖**- 60 miles per hour.

## NOTE :
- COT Prompting **work as step-by-step**, it **START** the process, then **THINK...** and finally generate the **OUTPUT**.

## Example COT Promting + Few-Shot Prompting :
```
# Chain Of Thought Prompting
from dotenv import load_dotenv
from openai import OpenAI

import json

load_dotenv()

client = OpenAI()


SYSTEM_PROMPT = """
    You're an expert AI Assistant in resolving user queries using chain of thought.
    You work on START, PLAN and OUPUT steps.
    You need to first PLAN what needs to be done. The PLAN can be multiple steps.
    Once you think enough PLAN has been done, finally you can give an OUTPUT.

    Rules:
    - Strictly Follow the given JSON output format
    - Only run one step at a time.
    - The sequence of steps is START (where user gives an input), PLAN (That can be multiple times) and finally OUTPUT (which is going to the displayed to the user).

    Output JSON Format:
    { "step": "START" | "PLAN" | "OUTPUT", "content": "string" }

    Example:
    START: Hey, Can you solve 2 + 3 * 5 / 10
    PLAN: { "step": "PLAN": "content": "Seems like user is interested in math problem" }
    PLAN: { "step": "PLAN": "content": "looking at the problem, we should solve this using BODMAS method" }
    PLAN: { "step": "PLAN": "content": "Yes, The BODMAS is correct thing to be done here" }
    PLAN: { "step": "PLAN": "content": "first we must multiply 3 * 5 which is 15" }
    PLAN: { "step": "PLAN": "content": "Now the new equation is 2 + 15 / 10" }
    PLAN: { "step": "PLAN": "content": "We must perform divide that is 15 / 10  = 1.5" }
    PLAN: { "step": "PLAN": "content": "Now the new equation is 2 + 1.5" }
    PLAN: { "step": "PLAN": "content": "Now finally lets perform the add 3.5" }
    PLAN: { "step": "PLAN": "content": "Great, we have solved and finally left with 3.5 as ans" }
    OUTPUT: { "step": "OUTPUT": "content": "3.5" }
    
"""

print("\n\n")

message_history = [
    { "role": "system", "content": SYSTEM_PROMPT },
]

user_query = input("👉🏻 ")
message_history.append({ "role": "user", "content": user_query })

while True:
    response = client.chat.completions.create(
        model="gpt-4o",
        response_format={"type": "json_object"},
        messages=message_history
    )

    raw_result = response.choices[0].message.content
    message_history.append({"role": "assistant", "content": raw_result})
    
    parsed_result = json.loads(raw_result)

    if parsed_result.get("step") == "START":
        print("🔥", parsed_result.get("content"))
        continue

    if parsed_result.get("step") == "PLAN":
        print("🧠", parsed_result.get("content"))
        continue

    if parsed_result.get("step") == "OUTPUT":
        print("🤖", parsed_result.get("content"))
        break

print("\n\n")
```


