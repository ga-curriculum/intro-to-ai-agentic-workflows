<h1>
  <span class="headline">Intro to AI Agentic Workflows</span>
  <span class="subhead">LangGraph Framework for Building AI Agents</span>
</h1>

**Learning Objective**: By the end of this lesson you'll be able to build AI Agents using the most popular open-source framework called LangGraph.

## Understanding Large Language Models (LLMs)
Before diving into AI agent frameworks, it's important to understand Large Language Models (LLMs) as they form the core intelligence of modern AI agents.

### What is a Large Language Model?
An LLM is a type of AI model that excels at **understanding and generating human language**. These models:
- Are trained on vast amounts of text data
- Learn patterns, structure, and nuance in language
- Typically consist of many millions of parameters
- Are usually built on the Transformer architecture

### Types of Transformers
There are 3 main types of transformer models:

1. **Encoders**
   - Take text as input and output dense representations
   - Example: BERT from Google
   - Use cases: Text classification, semantic search
   - Size: Millions of parameters

2. **Decoders**
   - Focus on generating new tokens sequentially
   - Example: Llama from Meta
   - Use cases: Text generation, chatbots, code generation
   - Size: Billions of parameters

3. **Seq2Seq (Encoder-Decoder)**
   - Combine encoder and decoder functionality
   - Example: T5, BART
   - Use cases: Translation, summarization
   - Size: Millions of parameters

### Popular LLMs
| **Model** | **Provider** |
|-----------|--------------|
| Deepseek-R1 | DeepSeek |
| GPT4 | OpenAI |
| Llama 3 | Meta |
| SmolLM2 | Hugging Face |
| Gemma | Google |
| Mistral | Mistral |

### How LLMs Work
LLMs operate through:
- **Token Prediction**: Predicting the next token based on previous tokens
- **Autoregression**: Output from one pass becomes input for the next
- **Attention Mechanism**: Identifying relevant context for predictions
- **Special Tokens**: Using markers to structure generation (e.g., EOS tokens)

### Using LLMs in AI Agents
LLMs serve as the "brain" of AI agents by:
- Interpreting user instructions
- Maintaining conversation context
- Defining action plans
- Deciding which tools to use

You can use LLMs through:
1. Local deployment (with sufficient hardware)
2. Cloud/API services (e.g., Hugging Face Inference API)

## The need for Agentic AI Orchestration
AI agents require structured workflows to function effectively. Without a framework to manage workflows, state, and decision-making, agents struggle with:

- **Lack of Memory** -- Agents forget previous interactions and cannot maintain context.
- **Unstructured Workflows** -- Traditional methods lack coordination, leading to inefficiencies.
- **Limited Multi-Agent Collaboration** -- AI agents need to interact, share information, and work together efficiently.

This is where an agentic AI orchestration framework like LangChain comes handy.

## An Introduction to LangChain Framework
LangChain is an open-source framework designed and developed by **LangChain Inc.** to simplify the development of applications powered by large language models (LLMs), allowing developers to easily integrate these models with external data sources to build complex NLP applications like chatbots and question-answering systems.

The "Lang" in LangChain likely refers to "language," signifying its core focus on working with natural language processing capabilities while the "Chain" represents the ability to link together multiple components (like different LLMs or data sources or processing steps) to create complex workflows. 

### Key Characteristics
- **Function**: It provides a set of tools and abstractions to streamline the process of building LLM-based applications, including prompt engineering, data access, and chaining different LLM operations together. 
- **Model-agnostic**: LangChain can work with various LLMs from different providers, like OpenAI's GPT-3, Hugging Face models, etc.

### Key Components
- **LangChain**:	A set of libraries that builds the foundation of LLM applications (chatbots, retrieval-based AI, autonomous agents).
- **LangSmith**:	A set of libraries that monitors, tests, and optimizes AI applications for performance and reliability.


## What is LangGraph?
LangGraph is a specialized library within the LangChain framework, introduced in January 2023, that extends LangChain Framework by introducing a graph-based execution model instead of linear chains. It allows us to build more intricate LLM applications using graph-based structures. "LangGraph" is a combination of "Language" and "Graph," essentially meaning a graphical representation of language-based processes or workflows, where each node in the graph represents a specific language-related action or function (like text generation, information retrieval, or decision making), and the edges define the flow between them. It allows us to construct agentic workflows that are:

- 🔄 **Flexible** -- Easily adaptable for different applications.
- 📈 **Scalable** -- Supports large-scale AI-driven workflows.
- 🧠 **Stateful** -- Retains memory across sessions for context-aware interactions.

LangGraph integrates with **LangChain** and **LangSmith** but can also function independently, making it a **versatile** choice for AI Agent development. LangGraph is so versatile that the name of the framework from LangChain Inc. has, de-facto, shifted from LangChain to LangGraph.

### Key Features of LangGraph 🔑**
-   🧠 **Memory Management** -- Retains information across agent interactions.
-   👥 **Human-in-the-loop Capabilities** -- Enables human intervention at critical decision points.
-   🔄 **Workflow Orchestration** -- Allows multiple AI agents to collaborate on tasks.
-   💾 **Persistence Layer** -- Saves state and enables checkpointing for recovery.
-   📊 **Scalability** -- Easily expands to handle complex, multi-step AI applications.

## **LangGraph Demo**: Building an AI Agent for Automated Research
Let's build an **AI Research Assistant** that:  
- Uses **LangChain** to **retrieve and summarize** web content.  
- Uses **LangGraph** to **orchestrate multi-step workflows** (search, summarize, refine).  
- Uses **LangSmith** to **monitor and debug** execution.  


### Step 0: Installation  
Before running the code snippets given below in your Jupyter notebook, install the required libraries using this command in your command prompt/terminal/bash interface:  
```bash
pip install langchain langchain-openai langchain-community langgraph langsmith
```

### Step 1: Import Required Libraries
This code initializes key libraries and sets up API keys for OpenAI and LangSmith. 
```python
import os
from langchain_openai import OpenAI
from langchain_community.tools import DuckDuckGoSearchRun
from langchain.chains import LLMChain
from langchain.prompts import PromptTemplate
from langgraph.graph import StateGraph, END
from langsmith import traceable  # Enables monitoring & debugging

# Set API Keys (Replace with your OpenAI API key)
os.environ["OPENAI_API_KEY"] = "your-openai-key"
os.environ["LANGCHAIN_API_KEY"] = "your-langsmith-key"
os.environ["LANGCHAIN_TRACING_V2"] = "true"  # Enable LangSmith tracing
```

### Step 2: Define Search and Summarization Tools
This code defines a tool for searching the web and a prompt-based summarization chain. 
```python
# Web Search Tool (DuckDuckGo)
search_tool = DuckDuckGoSearchRun()

# Summarization Prompt
summary_prompt = PromptTemplate(
    input_variables=["text"],
    template="Summarize the following information:\n{text}"
)

# Summarization Model (GPT)
summarizer = LLMChain(
    llm=OpenAI(model="gpt-3.5-turbo"),
    prompt=summary_prompt
)
```

### Step 3: Define Workflow Using LangGraph
This code creates a graph-based workflow with multiple steps: search → summarize → refine. 
```python
from langgraph.pregel import PregelGraph  # Graph-based execution

class ResearchState:
    """State class to hold query, search results, summary, and refined output."""
    def __init__(self, query):
        self.query = query
        self.search_results = None
        self.summary = None
        self.refined_output = None

# Define LangGraph Workflow
workflow = StateGraph(ResearchState)

@workflow.add_node()
@traceable  # Enables LangSmith tracing
def search(state):
    """Perform web search based on the query."""
    state.search_results = search_tool.run(state.query)
    return state

@workflow.add_node()
@traceable
def summarize(state):
    """Summarize the retrieved content."""
    state.summary = summarizer.run(state.search_results)
    return state

@workflow.add_node()
@traceable
def refine(state):
    """Refine the summary by making it more concise and relevant."""
    refinement_prompt = f"Refine this summary to be concise:\n{state.summary}"
    state.refined_output = summarizer.run(refinement_prompt)
    return state

# Define workflow structure
workflow.add_edge("search", "summarize")
workflow.add_edge("summarize", "refine")
workflow.set_entry_point("search")
workflow.set_finish_point("refine")
```

### Step 4: Run the Research Agent
This code executes the graph-based research agent for a given query.
```python
# Initialize the research workflow
research_agent = workflow.compile()

# Run the workflow with a sample query
query = "Latest advancements in quantum computing"
final_state = research_agent.invoke(ResearchState(query))

# Display final refined output
print("🔍 Query:", query)
print("\n📑 Search Results:", final_state.search_results[:300])  # Show first 300 chars
print("\n✍️ Summary:", final_state.summary[:300])  # Show first 300 chars
print("\n✅ Refined Output:", final_state.refined_output)
```

### Step 5: Monitor Execution Using LangSmith
LangSmith automatically tracks execution, but we can explicitly log traces.
```python
from langsmith import Client

client = Client()

# Fetch latest traces for debugging
traces = client.list_traces()
print(traces[-1])  # Show the latest trace
```

###  How LangChain, LangGraph, and LangSmith Work Together
| **Component** | **Role in This AI Research Agent** |
|--------------|------------------------------------|
| **LangChain** | Handles LLM integration for search and summarization. |
| **LangGraph** | Orchestrates multi-step workflows (search → summarize → refine). |
| **LangSmith** | Monitors, debugs, and optimizes execution. |


## Standard Design Process for Building an AI Agent using LangGraph (LangChain Framework)  
The process of designing an AI agent follows a structured pipeline that aligns with best practices in **AI agent development**. Now that we've already seen a demo of building an AI Agent, lets use the demo to understand the  standardized sequence of steps that can be applied when building **any AI agent**:

### Stage 1: Define the Agent's Goal & Use Case
- Clearly identify **what the AI agent should accomplish**.
- Example: *A research assistant that retrieves and summarizes web content*.

#### **Key Questions**:  
- What problem does the agent solve?  
- What input does it require?  
- What output should it generate?

### Stage 2: Select the Core Components
- **LLMs**: Choose a language model (e.g., GPT-4, LLaMA, Claude).  
- **Tools & APIs**: Identify external tools (e.g., web search, databases).  
- **Memory & Storage**: Determine whether the agent needs memory (short-term or long-term).  

#### Example Components from Demo:  
- **LLM**: OpenAI's GPT-3.5 Turbo  
- **Tool**: DuckDuckGo for web search  
- **Memory**: No long-term memory (stateless agent)

### Stage 3: Implement the Agent's Core Logic
- Break the **workflow into steps**.
- Implement functions for each step.  
- Use **LangChain** for LLM integration & tool execution.  

#### Example Steps from Demo:  
- **Search** → Retrieve information from the web.  
- **Summarize** → Process raw text into a concise summary.  
- **Refine** → Improve summary clarity & conciseness.  

### Stage 4: Orchestrate Workflow Using a Graph-Based Approach
- Use **LangGraph** or another framework for structuring the multi-step execution.
- Define **nodes (functions)** and **edges (flow between steps)**.
- Ensure the workflow has a **clear entry & exit point**.  

#### Example Graph from Demo:  
🔵 `search` → 🟢 `summarize` → 🟠 `refine`

### Stage 5: Enable Observability & Debugging
- Use **LangSmith** (or an equivalent tool) to monitor execution.
- Capture logs, visualize traces, and track errors.
- Optimize the agent's performance based on logs.  

#### Example Debugging Tools from Demo:  
- **LangSmith's traceable decorator** for tracking function calls.  
- **Fetching traces via LangSmith Client** for monitoring execution history.  

### Stage 6: Run, Test & Optimize the Agent
- Execute the agent with different inputs.
- Evaluate output quality using **human feedback or automated metrics**.
- Fine-tune the agent for better performance (e.g., improve prompt engineering).  

#### Example Validation Steps from Demo:  
- **Check if search results are relevant**.  
- **Ensure summarization is clear & coherent**.  
- **Refine output for better readability**.  


### Stage 7: Deploy & Scale the Agent
- Deploy the agent as an **API**, **chatbot**, or **autonomous system**.
- Optimize for **scalability, latency, and cost**.
- Monitor **real-world usage** and update the agent as needed.  

#### Example Deployment Scenarios:  
- **API-based integration** for chatbot assistants.  
- **Automated research tools** for knowledge workers.  
- **Internal workflow automation** for enterprises.  

This **structured process** can be applied to **any AI agent**, whether it's a **customer support bot, AI research assistant, or automation agent**.

##  Real-World Applications of LangGraph

LangGraph is already transforming industries by enabling **AI-driven automation**. Some notable applications include:

**🛍️ Retail & E-Commerce:**

-   🛒 **Personalized Shopping Assistants**\
AI-powered chatbots provide tailored product recommendations.

-   📦 **Inventory Management Agents**\
Track stock levels and automate restocking.

**📞 Customer Support:**

-   🤖 **Automated Helpdesks**\
AI agents resolve user queries while escalating complex issues to human representatives.

-   💬 **Multi-Agent Collaboration**\
Chatbots and voice assistants work together for seamless customer interactions.

**🏢 Business Process Automation:**

-   📜 **Legal Document Review**\
AI agents assist in reviewing and summarizing legal contracts.

-   💳 **Financial Transactions**\
Automate compliance checks and fraud detection.

## Key Takeaway
LangGraph provides a **structured and scalable solution** for AI-driven workflows, making it an essential tool for developers building intelligent applications. In the next lesson, we will explore **how to create an agent using LangGraph**.

## 🗣️ **Discussion Activity**
How would you design a workflow for an AI tutor that personalizes learning?