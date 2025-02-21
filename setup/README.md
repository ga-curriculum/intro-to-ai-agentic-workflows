<h1>
  <span class="headline">Intro to AI Agentic Workflows</span>
  <span class="subhead">Setup</span>
</h1>

## Launch the virtual environment

1.1 **Create and Navigate to the Project Directory:**

```bash
mkdir intro-to-ai-aegentic-workflows
cd intro-to-ai-aegentic-workflows
```

1.2 **Create a Virtual Environment:**

```bash
python3 -m venv env
```

1.3 **Activate the Virtual Environment:**

```bash
source env/bin/activate
```

1.4 **Create a requirements.txt file and add these packages:**

```txt
langchain-core>=0.1.0
langchain-community>=0.0.10
langchain-openai>=0.0.3
python-dotenv>=1.0.0
duckduckgo-search>=4.1.1
tabulate>=0.9.0
```

1.5 **Install Required Packages:**

```bash
pip install -r requirements.txt
```

## Setup

1. **Create a Directory**

   - On your machine, create a directory or folder named `intro-to-ai-agentic-workflows` where you can save all the data and the jupyter notebooks concerned with this module.
     ```sh
     jupyter notebook
     ```
     This will open Jupyter Notebook in your default web browser.

2. **Navigate to the Module Directory**

   - In the Jupyter Notebook interface, browse to the folder created in step 1. This is where you want to create your notebook.

3. **Create a New Jupyter Notebook**
   - Click **New** (top-right corner) → **Python 3** to create a new notebook.
   - Rename the notebook by clicking on the **default name ("Untitled")** and entering the name as:
     ```
     <INTRO_TO_AI_AGENTIC_WORKFLOWS>.ipynb
     ```
4. **Write and Execute Python Code**

   - You can copy the demo python code given in the lessons or tryout your own code too.
   - Click inside a code cell and paste the demo code or type your Python code.
   - Press **Shift + Enter** to run the code in the cell.

5. **Save and Close the Notebook**

   - Click **File → Save and Checkpoint** to save progress.
   - To close, select **File → Close and Halt**, then close the browser tab.

6. **Shut Down Jupyter Notebook**
   - In the terminal where Jupyter Notebook is running, press **Ctrl + C** and type `Y` when prompted.
