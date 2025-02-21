# Building Multi-Agent Workflows with LangChain
Hands-on lab using LangChain tools to build Math, SQL, and Search agents, focusing on real-world applications.

**Duration**: 90 minutes  
**Author**: Claudio Canales

---

## Table of Contents

- [Introduction](#introduction)
- [Learning Objectives](#learning-objectives)
- [Prerequisites](#prerequisites)
- [Environment Setup](#environment-setup)
- [Building a Math Agent](#building-a-math-agent)
- [Building a SQL Agent](#building-a-sql-agent)
- [Building a Search Agent](#building-a-search-agent)
- [Combining Agents into a Workflow](#combining-agents-into-a-workflow)
- [Practical Exercises](#practical-exercises)
- [Summary](#summary)
- [Additional Resources](#additional-resources)

---

## Introduction

Welcome to the lab on **Building Multi-Agent Workflows with LangChain**. In this session, we'll delve into the powerful capabilities of LangChain, a framework designed to simplify the creation of applications powered by language models.

We'll start by setting up our development environment and then proceed to build three specialized agents:

1. **Math Agent**: Performs mathematical computations.
2. **SQL Agent**: Interacts with a database.
3. **Search Agent**: Retrieves information from the web.

Finally, we'll combine these agents into a cohesive workflow, enabling them to work together to solve complex, multi-step tasks.

---

## Learning Objectives

By the end of this lab, you will be able to:

- ✅ Set up a development environment for building LangChain agents
- ✅ Create and customize a Math Agent for handling mathematical operations
- ✅ Develop a SQL Agent for database interactions
- ✅ Build a Search Agent for web information retrieval
- ✅ Combine multiple agents into a cohesive workflow

---

## Prerequisites

Before starting, ensure you have:

- **Python 3.7+** installed on your system
- Basic knowledge of **Python programming**
- Familiarity with **virtual environments** in Python
- An **OpenAI API key** (Sign up at [OpenAI's website](https://openai.com))
- Basic understanding of **SQL** and databases
- Internet access for web searches

---

## Environment Setup

Let's begin by setting up our development environment.

### 1. Virtual Environment Setup

Creating a virtual environment ensures that our project dependencies are isolated from other Python packages on your system.

Open your terminal or command prompt and execute the following commands:

```bash
# Create a new directory for the project
mkdir langchain_agents
cd langchain_agents

# Create a virtual environment named 'venv'
python -m venv venv

# Activate the virtual environment
# On Windows:
# venv\Scripts\activate
# On Unix or MacOS:
source venv/bin/activate

```

**Note**: Make sure you're using Python 3.6 or higher.

### 2. Install Required Packages

Create a `requirements.txt` file in the project root directory with the following content:

```txt
langchain-core>=0.1.0
langchain-community>=0.0.10
langchain-openai>=0.0.3
python-dotenv>=1.0.0
duckduckgo-search>=4.1.1
tabulate>=0.9.0
```

Install the packages by running:

```bash
python -m pip install -r requirements.txt
```

**Explanation**:

- `langchain-core`, `langchain-community`, `langchain-openai`: Core LangChain packages and integrations.
- `python-dotenv`: Loads environment variables from a `.env` file.
- `duckduckgo-search`: Allows us to perform web searches using DuckDuckGo.
- `tabulate`: Helps in displaying tabular data in a readable format.

### 3. Environment Variables

Create a `.env` file in your project root directory to securely store your API keys:

```env
OPENAI_API_KEY=your_api_key_here
```

**Replace `your_api_key_here` with your actual OpenAI API key**.

### 4. Basic Project Structure

Your project directory should look like this:

```plaintext
langchain_agents/
├── venv/
├── .env
├── requirements.txt
├── main.py
├── agents/
│   ├── __init__.py
│   ├── math_agent.py
│   ├── sql_agent.py
│   ├── search_agent.py
│   └── workflow.py
└── data/
    └── sample.db
```

**Explanation**:

- `venv/`: The virtual environment directory.
- `.env`: Contains environment variables.
- `requirements.txt`: Lists the project's dependencies.
- `main.py`: The main script to run and test agents.
- `agents/`: Directory for agent modules.
- `data/`: Directory for data files, like the sample database.

---

## Building a Math Agent

Our first task is to create a **Math Agent** capable of performing basic mathematical operations.

### Understanding the Math Agent

The Math Agent will:

- Interpret natural language queries involving mathematical operations.
- Use predefined tools to perform calculations.
- Return the computed results.

**Supported Operations**:

- Addition
- Subtraction
- Multiplication
- Division
- Square Root

### Implementing the Math Agent

**Step 1**: Create the `agents/` directory and `__init__.py` file if they don't exist.

```bash
mkdir agents
touch agents/__init__.py
```

**Step 2**: Create `agents/math_agent.py`.

```bash
touch agents/math_agent.py
```

**Step 3**: Implement the `MathAgent` class.

Open `agents/math_agent.py` and add the following code:

```python
"""
Mathematical Operations Agent using LangChain

This module implements a mathematical agent that can interpret natural language queries
and perform basic mathematical operations. The agent uses LangChain's framework to
process queries and execute appropriate mathematical functions.

Key Features:
- Natural language processing for mathematical operations
- Basic arithmetic operations (addition, subtraction, multiplication, division)
- Square root calculation
- Error handling for invalid operations (e.g., division by zero)

Usage:
    agent = MathAgent()
    result = agent.run("What is the square root of 16 plus 5?")

The agent will interpret the query, break down the operations, and execute them
in the correct order using the available mathematical tools.
"""

import os
import math
from dotenv import load_dotenv
from langchain_core.tools import Tool
from langchain_openai import ChatOpenAI
from langchain.agents import AgentType, initialize_agent

load_dotenv()  # Load environment variables from .env file

class MathAgent:
    def __init__(self, temperature=0):
        self.llm = ChatOpenAI(temperature=temperature)
        self.tools = self._create_tools()
        self.agent = initialize_agent(
            tools=self.tools,
            llm=self.llm,
            agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
            verbose=True
        )

    def _create_tools(self):
        def parse_two_numbers(input_str):
            try:
                # Remove any whitespace and split by comma
                numbers = [n.strip() for n in input_str.split(',')]
                if len(numbers) != 2:
                    raise ValueError("Input must contain exactly two numbers separated by a comma")
                # Convert to tuple of floats instead of map object
                return tuple(float(n) for n in numbers)
            except Exception as e:
                raise ValueError(f"Invalid input format. Expected two numbers separated by a comma. Error: {str(e)}")

        return [
            Tool(
                name="Addition",
                func=lambda input_str: sum(parse_two_numbers(input_str)),
                description="Adds two numbers together. Input should be two numbers separated by a comma."
            ),
            Tool(
                name="Subtraction",
                func=lambda input_str: parse_two_numbers(input_str)[0] - parse_two_numbers(input_str)[1],
                description="Subtracts the second number from the first. Input should be two numbers separated by a comma."
            ),
            Tool(
                name="Multiplication",
                func=lambda input_str: parse_two_numbers(input_str)[0] * parse_two_numbers(input_str)[1],
                description="Multiplies two numbers. Input should be two numbers separated by a comma."
            ),
            Tool(
                name="Division",
                func=lambda input_str: (
                    parse_two_numbers(input_str)[0] / parse_two_numbers(input_str)[1] 
                    if parse_two_numbers(input_str)[1] != 0 
                    else "Error: Division by zero"
                ),
                description="Divides the first number by the second. Input should be two numbers separated by a comma."
            ),
            Tool(
                name="Square_Root",
                func=lambda x: math.sqrt(float(x)) if float(x) >= 0 else "Error: Cannot calculate square root of a negative number",
                description="Calculates the square root of a number. Input should be a non-negative number."
            )
        ]

    def run(self, query):
        return self.agent.run(query)
```

### Testing the Math Agent

**Step 1**: Create `main.py` in the project root.

```bash
touch main.py
```

**Step 2**: Implement the testing code in `main.py`.

```python
from agents.math_agent import MathAgent

def test_math_agent():
    math_agent = MathAgent()
    
    test_queries = [
        "What is 25 plus 15?",
        "Calculate the square root of 16",
        "What is 30 divided by 6?",
        "Multiply 13 by 4"
    ]
    
    for query in test_queries:
        print(f"\nQuery: {query}")
        result = math_agent.run(query)
        print(f"Result: {result}")

if __name__ == "__main__":
    test_math_agent()
```

**Step 3**: Run the script.

```bash
python main.py
```

**Expected Output**:

The agent should process each query and return the correct mathematical result.

---

## Building a SQL Agent

Next, we'll create a **SQL Agent** to interact with a sample database.

### Setting Up the Sample Database

**Step 1**: Create the `data/` directory if it doesn't exist.

```bash
mkdir data
```

**Step 2**: Create `create_sample_db.py` in the project root.

```bash
touch create_sample_db.py
```

And add this code:

```python
"""
Creates a sample SQLite database for demonstration purposes.

This script initializes a SQLite database named 'sample.db' in the data directory
and populates it with an 'employees' table containing sample employee records.
Each record includes an ID, name, department, and salary information.

Key Features:
- Creates a SQLite database if it doesn't exist
- Sets up an employees table with basic employee information
- Populates the table with sample employee records
- Handles duplicate entries using INSERT OR REPLACE

Usage:
    Run this script directly to create and populate the database:
    $ python create_sample_db.py
"""

import sqlite3

def create_sample_database():
    conn = sqlite3.connect('data/sample.db')
    cursor = conn.cursor()
    
    # Create the employees table
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS employees (
            id INTEGER PRIMARY KEY,
            name TEXT,
            department TEXT,
            salary REAL
        )
    ''')
    
    # Insert sample data
    sample_data = [
        (1, 'John Doe', 'IT', 75000),
        (2, 'Jane Smith', 'HR', 65000),
        (3, 'Bob Johnson', 'IT', 80000),
        (4, 'Alice Brown', 'Marketing', 70000)
    ]
    
    cursor.executemany('INSERT OR REPLACE INTO employees VALUES (?, ?, ?, ?)', sample_data)
    conn.commit()
    conn.close()

if __name__ == "__main__":
    create_sample_database()
```

**Step 3**: Run the script to create the database.

```bash
python create_sample_db.py
```

**Step 4**: Verify that `data/sample.db` has been created.

### Implementing the SQL Agent

**Step 1**: Create `agents/sql_agent.py`.

```bash
touch agents/sql_agent.py
```

**Step 2**: Implement the `SQLAgent` class.

Open `agents/sql_agent.py` and add the following code:

```python
"""
SQL Agent - An AI-powered SQL query assistant

This module implements an intelligent SQL agent that can interpret natural language
queries and execute them against a SQLite database. The agent uses LangChain's
framework to process queries and return results.

Key Features:
- Natural language to SQL query conversion
- Safe SQL query execution
- Error handling and reporting
- Configurable temperature for response variation

Usage:
    agent = SQLAgent("path/to/database.db")
    result = agent.run("Show me all users who joined last month")

The agent will interpret the natural language request, convert it to SQL,
and return the query results from the database.
"""

import os
import sqlite3
from dotenv import load_dotenv
from langchain_core.tools import Tool
from langchain_openai import ChatOpenAI
from langchain.agents import AgentType, initialize_agent

# Load OpenAI API key and other environment variables
load_dotenv()

class SQLAgent:
    """
    A class that provides natural language interface to SQL databases.
    Uses LangChain's agent framework to convert natural language to SQL queries.
    """
    def __init__(self, db_path, temperature=0):
        """
        Initialize the SQL Agent with database connection and LLM configuration.
        
        Args:
            db_path: Path to the SQLite database file
            temperature: Controls randomness in LLM responses (0=deterministic, 1=creative)
        """
        self.db_path = db_path
        # Initialize ChatGPT model with specified temperature
        self.llm = ChatOpenAI(temperature=temperature)
        # Create tools that the agent can use
        self.tools = self._create_tools()
        # Initialize LangChain agent with zero-shot learning approach
        self.agent = initialize_agent(
            tools=self.tools,
            llm=self.llm,
            agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,  # Uses reasoning to determine action
            verbose=True  # Enables detailed logging of agent's thought process
        )

    def _create_tools(self):
        """
        Creates a set of tools that the agent can use to interact with the database.
        Currently implements a single tool for SQL query execution.
        
        Returns:
            List of Tool objects that the agent can utilize
        """
        def execute_query(query):
            """
            Executes a SQL query safely and returns the results.
            
            Args:
                query: SQL query string to execute
                
            Returns:
                String representation of query results or error message
            """
            conn = sqlite3.connect(self.db_path)
            cursor = conn.cursor()
            try:
                cursor.execute(query)
                results = cursor.fetchall()
                conn.close()
                return str(results)
            except Exception as e:
                conn.close()
                return f"Error: {str(e)}"

        # Define available tools for the agent
        return [
            Tool(
                name="SQLQuery",
                func=execute_query,
                description="Executes a SQL query on the database. Input should be a valid SQL query string."
            )
        ]

    def run(self, query):
        """
        Processes a natural language query and returns database results.
        
        Args:
            query: Natural language query string (e.g., "Show all users from New York")
            
        Returns:
            Query results or error message from the database
        """
        return self.agent.run(query)
```

**Explanation**:

- **`SQLAgent` Class**:
  - Connects to the SQLite database specified by `db_path`.
  - Provides a tool to execute SQL queries.
- **`execute_query` Function**:
  - Executes the SQL query and fetches results.
  - Handles exceptions and closes the database connection properly.

### Testing the SQL Agent

**Step 1**: Update `main.py` to test the SQL Agent. (We're creating a consolidated main.py at the end)

```python
from agents.sql_agent import SQLAgent

def test_sql_agent():
    sql_agent = SQLAgent('data/sample.db')
    
    test_queries = [
        "How many employees are in the IT department?",
        "What is the average salary of all employees?",
        "List all employees in the Marketing department."
    ]
    
    for query in test_queries:
        print(f"\nQuery: {query}")
        result = sql_agent.run(query)
        print(f"Result: {result}")

if __name__ == "__main__":
    test_sql_agent()
```

**Step 2**: Run the script.

```bash
python main.py
```

**Expected Output**:

The agent should interpret the natural language queries, translate them into SQL, execute them, and return the results.

---

## Building a Search Agent

Now, we'll build a **Search Agent** that retrieves information from the web.

### Understanding the Search Agent

The Search Agent will:

- Receive a query.
- Use DuckDuckGo's search API to fetch information (It's very common to get Rate Limit errors. Just wait and try again)
- Return the relevant results.

### Implementing the Search Agent

**Step 1**: Create `agents/search_agent.py`.

```bash
touch agents/search_agent.py
```

**Step 2**: Implement the `SearchAgent` class.

Open `agents/search_agent.py` and add the following code:

```python
"""
Search Agent Implementation

A LangChain-based agent that performs web searches using DuckDuckGo and processes results
using OpenAI's language model. The agent can interpret natural language queries and
execute web searches accordingly.

Key Features:
- Natural language query processing
- Web search capabilities via DuckDuckGo
- Error handling for failed queries
- Configurable temperature for response generation

"""

import os
from dotenv import load_dotenv
from langchain_core.tools import Tool
from langchain_openai import ChatOpenAI
from langchain.agents import AgentType, initialize_agent
from langchain_community.utilities import DuckDuckGoSearchAPIWrapper

load_dotenv()  # Load environment variables from .env file

class SearchAgent:
    def __init__(self, temperature=0):
        self.llm = ChatOpenAI(temperature=temperature)
        self.search = DuckDuckGoSearchAPIWrapper(
            region="wt-wt",
            safesearch="moderate",
            time="y"
        )
        self.tools = self._create_tools()
        self.agent = initialize_agent(
            tools=self.tools,
            llm=self.llm,
            agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
            verbose=True,
            handle_parsing_errors=True
        )

    def _create_tools(self):
        return [
            Tool(
                name="WebSearch",
                func=self.search.run,
                description="Searches the web for information. Input should be a search query string."
            )
        ]

    def run(self, query):
        try:
            return self.agent.run(query)
        except Exception as e:
            return f"Error processing query: {str(e)}"
```

**Explanation**:

- **`SearchAgent` Class**:
  - Uses DuckDuckGo's API to perform web searches.
  - Provides a tool named `WebSearch`.

### Testing the Search Agent

**Step 1**: Update `main.py` to test the Search Agent.

```python
from agents.search_agent import SearchAgent
import time

def test_search_agent():
    search_agent = SearchAgent()
    
    test_queries = [
        "What is the capital of France?",
        "Who won the Nobel Prize in Literature in 2023?",
        "Latest Copa Libertadores winner."
    ]
    
    for query in test_queries:
        print(f"\nQuery: {query}")
        result = search_agent.run(query)
        print(f"Result: {result}")
        
        # Stop if we hit the rate limit
        if "rate limit reached" in result.lower():
            print("\nStopping tests due to rate limit...")
            break
            
        # Add a 5-second delay between queries
        time.sleep(5)

if __name__ == "__main__":
    test_search_agent()
```

**Step 2**: Run the script.

```bash
python main.py
```

**Expected Output**:

The agent should return relevant information for each query.

---

## Combining Agents into a Workflow

With our individual agents ready, it's time to combine them into a cohesive workflow.

### Implementing the Agent Workflow

**Step 1**: Create `agents/workflow.py`.

```bash
touch agents/workflow.py
```

**Step 2**: Implement the `AgentWorkflow` class.

Open `agents/workflow.py` and add the following code:

```python
"""
Agent Workflow Manager

This module provides a workflow management system for coordinating different types of AI agents.
Each agent specializes in specific tasks:
- Math Agent: Handles mathematical calculations and problems
- SQL Agent: Executes database queries and operations
- Search Agent: Performs information retrieval tasks

Key Features:
- Task routing to specialized agents
- Sequential workflow execution
- Support for multiple task types in a single workflow
- Error handling for unknown task types

Usage:
    workflow = AgentWorkflow(db_path="path/to/database")
    
    # Single task execution
    task = {"type": "math", "query": "Calculate 2 + 2"}
    result = workflow.process_task(task)
    
    # Multiple task workflow
    tasks = [
        {"type": "sql", "query": "SELECT * FROM users"},
        {"type": "search", "query": "Latest AI developments"}
    ]
    results = workflow.run_workflow(tasks)
"""

from typing import List, Dict
from .math_agent import MathAgent
from .sql_agent import SQLAgent
from .search_agent import SearchAgent

class AgentWorkflow:
    """
    Orchestrates the execution of tasks across different specialized agents.
    Acts as a facade pattern, providing a simplified interface to the complex
    subsystem of different agents.
    """
    def __init__(self, db_path: str):
        """
        Initialize all agent types with necessary configurations.
        
        Args:
            db_path: Path to the database file required by SQLAgent
        """
        self.math_agent = MathAgent()
        self.sql_agent = SQLAgent(db_path)
        self.search_agent = SearchAgent()
        
    def process_task(self, task: Dict):
        """
        Routes individual tasks to appropriate specialized agents based on task type.
        Implements a strategy pattern where each agent handles a specific task type.
        
        Args:
            task: Dictionary containing:
                - type: The type of task ('math', 'sql', or 'search')
                - query: The actual query/problem to be solved
        
        Returns:
            The result from the appropriate agent or an error message if validation fails
        
        Note:
            Task types are case-insensitive for better usability
        """
        task_type = task.get('type', '').lower()
        query = task.get('query', '')
        
        # Early validation to prevent processing empty queries
        if not query:
            return "Error: No query provided"
            
        # Route task to appropriate agent based on type
        if task_type == 'math':
            return self.math_agent.run(query)
        elif task_type == 'sql':
            return self.sql_agent.run(query)
        elif task_type == 'search':
            return self.search_agent.run(query)
        else:
            return f"Error: Unknown task type '{task_type}'"
            
    def run_workflow(self, tasks: List[Dict]):
        """
        Executes a sequence of tasks in order, maintaining a history of both
        tasks and their results for traceability.
        
        Args:
            tasks: List of task dictionaries, each containing 'type' and 'query' keys
        
        Returns:
            List of dictionaries containing both the original task and its result,
            allowing for full audit trail of the workflow execution
        
        Note:
            Tasks are processed sequentially; no parallel execution is implemented
        """
        results = []
        for task in tasks:
            print(f"\nProcessing task: {task}")  # Logging for debugging/monitoring
            result = self.process_task(task)
            # Store both task and result for complete workflow history
            results.append({
                'task': task,
                'result': result
            })
        return results
```

**Explanation**:

- **`AgentWorkflow` Class**:
  - Initializes instances of all three agents.
  - `process_task`: Determines which agent to use based on the task type.
  - `run_workflow`: Processes a list of tasks and collects results.

### Testing the Complete Workflow

**Step 1**: Update `main.py` to test the complete workflow.

```python
"""
Multi-Agent Workflow Executor

This program demonstrates a workflow system that can execute different types of tasks
using specialized agents. It supports:
- Mathematical computations
- SQL database queries
- Web search operations

The workflow takes a sequence of tasks and processes them sequentially, returning
results for each operation.
"""

from agents.workflow import AgentWorkflow

def test_workflow():
    workflow = AgentWorkflow('data/sample.db')
    
    # Define a sequence of tasks
    tasks = [
        {
            'type': 'math',
            'query': 'What is the square root of 144?'
        },
        {
            'type': 'sql',
            'query': 'How many employees are in the IT department?'
        },
        {
            'type': 'search',
            'query': 'What is the capital of France?'
        }
    ]
    
    # Run the workflow
    results = workflow.run_workflow(tasks)
    
    # Print results
    for result in results:
        print("\nTask:", result['task'])
        print("Result:", result['result'])

if __name__ == "__main__":
    test_workflow()
```

**Step 2**: Run the script.

```bash
python main.py
```

**Expected Output**:

- **Math Task**: Should return 12 as the square root of 144.
- **SQL Task**: Should return the number of employees in the IT department.
- **Search Task**: Should return "Paris" as the capital of France.

---

## Practical Exercises

Now, it's your turn to extend and enhance the agents.

### 1. Math Agent Exercise

**Objective**: Extend the Math Agent to handle more complex operations.

**Tasks**:

- Implement percentage calculations.
- Implement cube root calculations.

**Instructions**:

1. **Add New Tools**: Modify the `MathAgent` class in `math_agent.py` to include:

   - **Percentage**:
     ```python
     Tool(
         name="Percentage",
         func=lambda x, y: (float(x) * float(y)) / 100,
         description="Calculates the percentage of a number. Input should be two numbers: value and percentage."
     )
     ```
   - **Cube Root**:
     ```python
     Tool(
         name="Cube_Root",
         func=lambda x: float(x) ** (1/3),
         description="Calculates the cube root of a number. Input should be a number."
     )
     ```

2. **Test the New Operations**: Update `main.py` to include:

   ```python
   tasks = [
       {'type': 'math', 'query': 'Calculate 15% of 200'},
       {'type': 'math', 'query': 'What is the cube root of 27?'}
   ]
   ```

3. **Run the Tests**:

   ```bash
   python main.py
   ```

### 2. SQL Agent Exercise

**Objective**: Write queries to analyze employee data.

**Tasks**:

- Query the average salary by department.
- Find the highest-paid employee in each department.

**Instructions**:

1. **Update the Database**: Ensure your `sample.db` has sufficient data for meaningful results.

2. **Test Queries**:

   ```python
   tasks = [
       {'type': 'sql', 'query': 'What is the average salary by department?'},
       {'type': 'sql', 'query': 'Find the highest paid employee in each department'}
   ]
   ```

3. **Handle SQL Translation**: Ensure the agent correctly translates natural language queries into SQL.

### 3. Search Agent Exercise

**Objective**: Combine search with data analysis.

**Tasks**:

- Retrieve and display the top programming languages in 2024.

**Instructions**:

1. **Update `main.py`**:

   ```python
   tasks = [
       {'type': 'search', 'query': 'What are the top programming languages in 2024?'},
       {'type': 'search', 'query': 'Latest developments in AI for 2024'}
   ]
   ```

2. **Enhance the Agent**: Modify `SearchAgent` to better handle and summarize longer responses if necessary.

3. **Run the Tests**:

   ```bash
   python main.py
   ```

---

## Summary

In this lab, you have:

- **Set up a development environment** for LangChain agents.
- **Created three specialized agents**:
  - **Math Agent**: Performs mathematical operations.
  - **SQL Agent**: Interacts with a database.
  - **Search Agent**: Retrieves information from the web.
- **Combined the agents into a workflow**, allowing them to collaborate on tasks.
- **Practiced extending the agents' capabilities** through practical exercises.

This lab serves as a foundation for building more complex, agent-based systems. You can further enhance this project by:

- **Adding more mathematical operations** to the Math Agent.
- **Expanding the SQL Agent's capabilities** with more complex queries and data handling.
- **Improving the Search Agent** to better process and summarize web content.
- **Implementing error handling and validation** across all agents.
- **Integrating additional agents** for tasks like data visualization or natural language processing.

---

## Additional Resources

- **LangChain Documentation**: [LangChain Docs](https://python.langchain.com/docs/tutorials/)
- **OpenAI API Documentation**: [OpenAI API Docs](https://platform.openai.com/docs/introduction)
- **SQLite Documentation**: [SQLite Docs](https://www.sqlite.org/docs.html)
- **Python `math` Module**: [Python Math Module](https://docs.python.org/3/library/math.html)
- **DuckDuckGo Search PIP Package**: [DuckDuckGo API](https://github.com/deedy5/duckduckgo_search)

---

**Congratulations!** You've successfully completed the lab on building multi-agent workflows with LangChain.