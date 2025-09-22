# LangGrph :
- LangGraph is a framework for building more reliable, maintainable, and flexible AI agent/workflow systems using graph‑based orchestration, strong support for state, human oversight, durability.

- LangGraph is a recently developed framework by LangChain (makers of LangChain) aimed at building more powerful, reliable, and flexible AI agents and workflows.

- LangGraph is built on top of LangChain, but adds something powerful: memory and loops.

## It helps you :
- **You can loop through steps.**

- **You can go back, try again, or decide based on previous output.**

- **It’s more like building a brain flowchart than a straight path.**

## Example :
### Imagine a chatbot that keeps asking for clarification until it fully understands your question.
- In LangChain, this is hard.
- In LangGraph, it’s easy — you just create a loop node:
- Ask → Understand → (If not clear → Ask again) → Answer

## LangGraph has 2 things which helps in **workflow** :
### Nodes :
- Nodes are **function** which performs specific task.

### Edges :
- Edges is used To **make connection with nodes**.

![](assets/langgraph.png)

## Example :
```
from dotenv import load_dotenv
from typing_extensions import TypedDict
from typing import Optional, Literal
from langgraph.graph import StateGraph, START, END
from openai import OpenAI
import os

load_dotenv()

client = OpenAI(api_key=os.getenv("OPEN_API_KEY"))

class State(TypedDict):
    user_query: str
    llm_output: Optional[str]
    is_good: Optional[bool]

def chatbot(state: State):
    print("ChatBot Node", state)
    response = client.chat.completions.create(
        model="gpt-4.1-mini",
        messages=[
            { "role": "user", "content": state.get("user_query") }
        ]
    )

    state["llm_output"] = response.choices[0].message.content
    return state

def evalaute_response(state: State) -> Literal["chatbot_gemini", "endnode"]:
    print("evalaute_response Node", state)

    # Build evaluation prompt
    evaluation_prompt = f"""
You are an AI assistant tasked with evaluating the quality of another AI's response.

USER PROMPT:
{state['user_query']}

AI RESPONSE:
{state['llm_output']}

QUESTION:
Is the response factually accurate, clear, and complete?

Respond with only "yes" or "no".
"""

    eval_response = client.chat.completions.create(
        model="gpt-4.1",
        messages=[
            {"role": "user", "content": evaluation_prompt}
        ]
    )

    decision = eval_response.choices[0].message.content.strip().lower()
    print("Evaluation decision:", decision)

    if "yes" in decision:
        return "endnode"
    else:
        return "chatbot_gemini"


def chatbot_gemini(state: State):
    print("chatbot_gemini Node", state)
    response = client.chat.completions.create(
        model="gpt-4.1",
        messages=[
            { "role": "user", "content": state.get("user_query") }
        ]
    )

    state["llm_output"] = response.choices[0].message.content
    return state

def endnode(state: State):
    print("endnode Node", state)
    return state

graph_builder = StateGraph(State)

# adding the Nodes
graph_builder.add_node("chatbot", chatbot)
graph_builder.add_node("chatbot_gemini", chatbot_gemini)
graph_builder.add_node("endnode", endnode)

# connecting the nodes
graph_builder.add_edge(START, "chatbot")
graph_builder.add_conditional_edges("chatbot", evalaute_response)
graph_builder.add_edge("chatbot_gemini", "endnode")
graph_builder.add_edge("endnode", END)

graph = graph_builder.compile()

updated_state = graph.invoke(State({"user_query": "Hey, What is 2 + 2?"}))
print(updated_state)

```