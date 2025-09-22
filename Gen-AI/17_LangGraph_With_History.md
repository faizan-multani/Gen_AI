# LangGraph With History :
- **To save history or last message, we will use mongodb with docker.**

- **In this code ,i used checkpointer with the connection of mongodb with docker.**

- **Make sure your docker is running.**
- **Make sure you configured the docker-compose.yml file correctly**.
- **Pull the docker-compose.yml file -> docker compose up -d**

```
from dotenv import load_dotenv
from typing_extensions import TypedDict
from typing import Annotated
from langgraph.graph.message import add_messages
from langgraph.graph import StateGraph, START, END
from langchain.chat_models import init_chat_model
from langgraph.checkpoint.mongodb import MongoDBSaver
import os

load_dotenv()

llm = init_chat_model(
    api_key=os.getenv("OPEN_API_KEY"),
    model="gpt-4.1-mini",
    model_provider="openai"
)

class State(TypedDict):
    messages: Annotated[list, add_messages]

def chatbot(state: State):
    response = llm.invoke(state.get("messages"))
    return { "messages": [response] }


graph_builder = StateGraph(State)

graph_builder.add_node("chatbot", chatbot)

graph_builder.add_edge(START, "chatbot")
graph_builder.add_edge("chatbot", END)

def compile_graph_with_checkpointer(checkpointer):
    return graph_builder.compile(checkpointer=checkpointer)

DB_URI = "mongodb://admin:admin@localhost:27017"
with MongoDBSaver.from_conn_string(DB_URI) as checkpointer:
    graph_with_checkpointer = compile_graph_with_checkpointer(checkpointer=checkpointer)

    config = {
            "configurable": {
                "thread_id": "faizan" # user_id
            }
        }


    for chunk in graph_with_checkpointer.stream(
        State({"messages": ["what is my name?"]}),
        config,
        stream_mode="values"
        ):
            chunk["messages"][-1].pretty_print()
    

# (START) -> chatbot -> (END)

# state = { messages: ["Hey there"] }
# node runs: chatbot(state: ["Hey There"]) -> ["Hi, This is a message from ChatBot Node"]

# Checkpointer (faizan) = Hey, My name is Faizan khan

```