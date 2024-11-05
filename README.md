# Enola-AI Python Library Documentation

## Table of Contents

1. [Introduction](#1-introduction)
2. [Features](#2-features)
3. [Requirements](#3-requirements)
4. [Installation](#4-installation)
5. [Getting Started](#5-getting-started)
   - 5.1. [Initializing Tracking](#51-initializing-tracking)
   - 5.2. [Track Activity: Basic Architecture](#52-track-activity-basic-architecture)
   - 5.3. [Understanding Steps in Enola-AI](#53-understanding-steps-in-enola-ai)
     - [Generic Steps](#generic-steps)
     - [LLM Steps](#llm-steps)
   - 5.4. [Feedback Evaluation](#54-feedback-evaluation)
   - 5.5. [Get Tracking: Extracting Information](#55-get-tracking-extracting-information)
6. [Documentation](#6-documentation)
7. [Summary](#7-summary)
8. [Contributing](#8-contributing)
9. [License](#9-license)
10. [Contact](#10-contact)

---

## 1. Introduction

Enola AI is an advanced GenAI platform designed to validate and monitor the robustness of artificial intelligence models in highly regulated industries such as finance, healthcare, and education. Our solution ensures that AI implementations comply with strict regulatory standards through continuous assessments, seamless integrations, and real-time monitoring.

This documentation provides a comprehensive guide on how to use the Enola-AI Python library to interact with the platform. You will learn how to install the library, understand the types of steps used in tracking, send different types of data, and utilize various features to enhance your AI model's performance and compliance.

---

## 2. Features

- **Multilevel Evaluation**: Collect feedback from users, automated assessments, and reviews from internal experts.
- **Real-Time Monitoring**: Continuous monitoring capabilities to detect deviations in AI model behavior.
- **Seamless Integration**: Compatible with existing infrastructures such as ERP systems, CRM platforms, and data analytics tools.
- **Customizable Configuration**: Adapt the evaluation methodology according to the specific needs of the client.
- **Security and Compliance**: Advanced security measures and compliance with data protection regulations.

---

## 3. Requirements

- **Python 3.7+**
- **Enola API Token**

---

## 4. Installation

Before installing the Enola-AI Python library, ensure that you have Python 3.7 or higher installed.

**Install the SDK via pip**

In your Command Prompt (Windows) or Terminal (Linux/macOS) type:

   ```bash
   pip install enola
   ```

By doing a pip install, you have the Enola-AI Python library and its dependencies automatically installed on your device.

**Note: Advanced users may prefer to use a Virtual Environment to manage dependencies and 
isolate the Enola-AI library from other Python packages.**

---

## 5. Getting Started

To start using the Enola-AI Python library, follow the steps below to initialize tracking in your application.

### 5.1. Initializing Tracking
To connect to Enola and initialize tracking you will need:
- A token provided by Enola-AI. This token is essential for authentication and authorization purposes
- A Python script. You can start by creating an empty Python file with a .py extension (eg. enola_sample.py, enola_script.py)

#### **1. Loading Enola API Token**
**You can load the token from a `.env` file (recommended):**

This method is recommended due to better security for token management.
- Go to the same directory where your Python file is located
- To load the token, create a file named `.env` in the same directory as your Python script

- Then open the file with a text editor and add this line:

    ```plaintext
    ENOLA_TOKEN=your_api_token
    ```
- Replace `your_api_token` with your Enola API token.
- Assuming the Python file and the `.env` file are in the same directory, your token should be loaded

**Alternatively, you can set it directly in your script:**

This method is easier and fine for testing purposes, but it is not recommended because the token is exposed in the script.

To set it directly in your Python script:
```python
token = 'your_api_token'
```
**You can also load the token using Environment Variables:**

Another method to load the token is by setting your Enola API token as an Environment Variable. This method (recommended for advanced users) offers more flexibility but requires more complex configurations, depending on your operating system.
For convenience, the method for loading the token from a `.env` file will be covered instead in this guide.


#### **2. Import the Necessary Libraries**
Assuming you are loading the token from the `.env` file,
In your Python script, start importing the necessary libraries:
```python
# Import necessary libraries
from enola.tracking import Tracking     # Enola Tracking module
from dotenv import load_dotenv          # .env Loader
import os                               # Import the os module for environment variable access
```

#### **3. Define User Input**
```python
# Define the user input message
user_input = "Hello, what can you do?"  # Input message from the user
```

#### **4. Load the Enola API token**
Load the Enola API token
```python
# Load .env file
load_dotenv()
# Set up your token
token = os.getenv('ENOLA_TOKEN')
```

#### **5. Initialize the Tracking Agent**

```python
# Initialize the tracking agent
monitor = Tracking(
    token=token,			  # Your Enola API token
    name="My Enola Project",  # Name of your tracking session
    is_test=True,             # Set to True if this is a test session
    app_id="my_app_id_01",    # Application ID
    user_id="user_123",       # User ID
    session_id="session_456", # Session ID
    channel_id="console",     # Channel ID (e.g., 'console', 'web', 'mobile')
    ip="192.168.1.1",         # IP address of the client
    message_input=user_input  # Input message from the user
)
```

#### **6. Create a New Step**

```python
# Create a step
step_chat = monitor.new_step("User LLM Question")
```

#### 7. **Add Extra Information**

Add any additional information relevant to the step, such as the user's question.

```python
# Add user's question
step_chat.add_extra_info("UserQuestion", user_input)
```

#### 8. **Process the User Input with the Language Model**

Simulate the model generating a response to the user's question.

```python
# Simulated model response
model_response = "I'm here to assist you in finding the help or information you need"
```

**Note**: You can replace the simulated response with an actual model response (e.g. GPT-4, Ollama, BERT).

#### 9. **Add the Model's Response to the Step**
```python
# Add model's response
step_chat.add_extra_info("ModelResponse", model_response)
```

#### 10. **Close the LLM Step**

Indicate that the step has completed successfully, and include token usage and costs.

```python
# Close the LLM Step with close_step_token
monitor.close_step_token(
    step=step_chat,
    successfull=True,
    message_output=model_response,
    token_input_num=12,       # Number of input tokens (estimated)
    token_output_num=15,      # Number of output tokens (estimated)
    token_total_cost=0.0025,  # Total cost (example)
    token_input_cost=0.001,   # Cost for input tokens
    token_output_cost=0.0015  # Cost for output tokens
)
```
#### 11. **Execute the Tracking**

Send the tracking data to the Enola-AI server.

```python
# Execute the tracking and send the data to Enola-AI server
monitor.execute(
    successfull=True,
    message_output=model_response,
    num_iteratons=1
)
```
---

#### **Complete Example: Basic Tracking Initialization**

```python
# Import necessary libraries
from enola.tracking import Tracking
from dotenv import load_dotenv
import os

# Define the user input message
user_input = "Hello, what can you do?"  # Input message from the user

# Load .env file and set up your token
load_dotenv()
token = os.getenv('ENOLA_TOKEN')

# Initialize the tracking agent
monitor = Tracking(
    token=token,
    name="My Enola Project",  # Name of your tracking session
    is_test=True,             # Set to True if this is a test session
    app_id="my_app_id_01",    # Application ID
    user_id="user_123",       # User ID
    session_id="session_456", # Session ID
    channel_id="console",     # Channel ID (e.g., 'console', 'web', 'mobile')
    ip="192.168.1.1",         # IP address of the client
    message_input=user_input  # Input message from the user
)

# Create a step
step_chat = monitor.new_step("User LLM Question")

# Add user's question
step_chat.add_extra_info("UserQuestion", user_input)

# Simulated model response
model_response = "I'm here to assist you in finding the help or information you need"

# Close the LLM Step
monitor.close_step_token(
    step=step_chat,
    successfull=True,
    message_output=model_response,
    token_input_num=12,
    token_output_num=15,
    token_total_cost=0.0025,
    token_input_cost=0.001,
    token_output_cost=0.0015
)

# Execute the tracking and send the data to Enola-AI server
monitor.execute(
    successfull=True,
    message_output=model_response,
    num_iteratons=1
)
```
After initializing the tracking agent and executing it, you should get a console output like this:
```plaintext
2024-10-30 09:43:29,909 WELCOME to Enola...
2024-10-30 09:43:29,909 authorized...  
2024-10-30 09:43:29,909 STARTED!!!     
My Enola Project: sending to server... 
My Enola Project: finish OK! 
```
**This means you have successfully connected to Enola-AI and sent the data to the servers.**

---

## 5.2. Track Activity

In a basic AI solution, you would use this architecture:
![Basic Architecture](docs/images/basic_architecture.jpg)

Basic architecture with AI solution:
- The User sends and Input.
- The Agent returns an Output.

When using Enola-AI, you can add Track Activity, allowing you to track any interactions that the system is doing.
![Track Activity](docs/images/track_activity.jpg)

Example Track Activity in Python:
![Track Activity Code](docs/images/track_activity_code.jpg)
Code explanation: 
1. You initialize the Tracking.
2. You validate the user input with Step 1.
3. You ask a question to an LLM Agent with Step 2.
4. You validate the model response with Step 3.
5. You send the data to Enola-AI.

Basic architecture with Enola-AI:
- The User sends and Input.
- The Agent returns an Output.
- Track Activity is sent to Enola-AI.

After doing the Tracking, you can check it in the Enola-AI platform:
![Track Activity Frontend](docs/images/track_activity_frontend.jpg)

By tracking the interactions in your system with Enola-AI, you can effectively monitor, validate and evaluate your models.

---

## 5.3. Understanding Steps in Enola-AI

In Enola-AI, the concept of **steps** is fundamental for tracking the execution flow of your AI agents. Each step represents a significant action or event in your agent's processing pipeline.

There are two main types of steps:

- **Generic Steps**: Used for general-purpose tracking of actions that are not specific to language models, such as data retrieval, preprocessing, or any custom logic.

- **LLM Steps**: Specifically designed for tracking interactions with Language Models (e.g., GPT-4, Ollama, BERT), where token usage and costs are relevant.

Understanding the difference between these step types is crucial for accurate tracking and cost analysis.

#### Generic Steps

Generic steps are used to track any action or process in your agent that doesn't involve a language model. This could include data retrieval, preprocessing, API calls, or custom computations.

**When to use Generic Steps:**

- Data fetching from databases or APIs.
- Data preprocessing and evaluation.
- Custom computations or business logic.
- Any step where token usage is not applicable.

#### LLM Steps

LLM Steps are used to track interactions with language models where token usage, input/output messages, and costs are important.

**When to use LLM Steps:**

- Sending prompts to a language model.
- Receiving responses from a language model.
- Tracking token counts and associated costs.

#### How to create and close Steps:
In your Python code, you can create and close steps.

**How to create a Step:**
```python
# Create a step
step_chat = monitor.new_step("New step creation example")
```

**How to close a Step as Generic:**
```python
# Close Step as Generic with monitor.close_step_others
monitor.close_step_others(
    step=example_generic_step,
    successfull=True,
    message_output=model_response
)
```

**How to close a Step as LLM:**

```python
# Close Step as LLM with monitor.close_step_token
monitor.close_step_token(
    step=example_llm_step,
    successfull=True,
    message_output=model_response,
    token_input_num=12,
    token_output_num=15,
    token_total_cost=0.0025,
    token_input_cost=0.001,
    token_output_cost=0.0015
)
```
---
## 5.4. Feedback Evaluation 

Enola-AI provides a feedback system that allows users to evaluate AI agent executions. Feedback can be submitted either through the Enola platform or programmatically via code. This helps in assessing the performance of your AI agents and gathering user insights for improvement.

![Feedback Evaluation](docs/images/feedback_evaluation.jpg)

By incorporating this feedback mechanism, you can effectively monitor and improve your AI agents based on user evaluations.


### Submitting Feedback via Code

You can submit feedback using the Enola-AI SDK in Python. Here's a simple example:

### Step 1: Import Necessary Modules

```python
from enola import evaluation
from enola.enola_types import EvalType
from dotenv import load_dotenv
import os
```

### Step 2: Set Your Enola Token

```python
load_dotenv()
token = os.getenv("ENOLA_TOKEN")
```

### Step 3: Create an Evaluation Instance

Specify the evaluation type (`USER`, `AUTO`, or `INTERNAL`) and user information.

```python
eval = evaluation.Evaluation(
    token=token,
    eval_type=EvalType.USER,
    user_id="user_123",
    user_name="John Doe",
)
```

### Step 4: Add an Evaluation

Provide the execution ID (`enola_id`), a unique evaluation ID (`eval_id`), an evaluation `value` between 0 and 100, and a comment.

```python
eval.add_evaluation(
    enola_id="8ed16f7d44263053c7fdbce834acf152",
    eval_id="001",
    value=85,  # Score between 0 and 100
    comment="The response was informative and well-structured."
)
```

### Step 5: Add an Evaluation by Level

You can also provide an evaluation by level instead of a value. The evaluation by `level` has a value between 1 (Very Bad) and 5 (Very Good).

```python
eval.add_evaluation_by_level(
    enola_id="8ed16f7d4426303eb86789e6e5429d11667fed7f456a7053c7fdbce834acf152",
    eval_id="002",
    level=4,
    comment="Good response, helpful and concise."
)
```

### Step 6: Execute the Evaluation Submission

Submit the evaluation to the Enola-AI platform.

```python
result = eval.execute()
```

### Complete Example
```python
# Import libraries
from enola import evaluation
from enola.enola_types import EvalType
from dotenv import load_dotenv
import os

# Set Your Enola Token
load_dotenv()
token = os.getenv("ENOLA_TOKEN")

# Create an Evaluation Instance
eval = evaluation.Evaluation(
    token=token,
    eval_type=EvalType.USER,
    user_id="user_123",
    user_name="John Doe",
)

# Add an Evaluation
eval.add_evaluation(
    enola_id="8ed16f7d4426303eb86789e6e5429d11667fed7f456a7053c7fdbce834acf152",
    eval_id="001",
    value=85,  # Score between 0 and 100
    comment="The response was informative and well-structured."
)

# Add an Evaluation by Level
eval.add_evaluation_by_level(
    enola_id="8ed16f7d4426303eb86789e6e5429d11667fed7f456a7053c7fdbce834acf152",
    eval_id="002",
    level=4,   # Score between 1 and 5
    comment="Good response, helpful and concise."
)

# Execute the Evaluation Submission
result = eval.execute()
```

### Notes

- **Evaluation Types**:
  - `EvalType.USER`: Feedback provided by end-users.
  - `EvalType.AUTO`: Automatic evaluations generated by the system.
  - `EvalType.INTERNAL`: Internal evaluations by developers or administrators.
- **Evaluation Values**: The `value` should be a number between 0 and 100.
- **Evaluation by Level Values**: The `level` should be a number between 1 and 5.
- **Evaluation IDs**: Use unique `eval_id`s for multiple evaluations.
- **Execution ID**: The `enola_id` is the identifier of the execution you are evaluating.

### Viewing Feedback on the Enola Platform

After submitting feedback, you can log in to the [Enola-AI platform](https://enola-ai.com/) to view and manage your evaluations.

---
## 5.5. Get Tracking: Extracting Information

Enola-AI allows you to retrieve the Input and Output data from past executions for analysis or further processing. 
![Get Tracking](docs/images/get_tracking.jpg)

Here's a simple example demonstrating how to extract information using Python.

### Example Code

```python
# Import necessary libraries
from enola import get_executions
from enola.enola_types import ExecutionEvalFilter
from dotenv import load_dotenv
import os

# Load .env file with token inside
load_dotenv()

# Set up your Enola token
token = os.getenv("ENOLA_TOKEN")

# Create a GetExecutions instance
exec = get_executions.GetExecutions(token=token, raise_error_if_fail=False)

# Define your query parameters
exec.query(
    date_from="2024-10-28T00:00",
    date_to="2024-12-31T23:59",
    limit=20,
    eval_id_internal=ExecutionEvalFilter(
        eval_id=["0"],
        include=False  # Retrieve executions that have not been internally evaluated
    )
)

# Retrieve and process data
while exec.continue_execution and exec.get_page_number() < 70:
    result_data = exec.get_next_page()
    print("Page:", exec.get_page_number(), ", Data Length:", len(result_data.data))

    if len(result_data.data) == 0:
        break

    for row in result_data.data:
        # Extract and print details of each execution
        print(f"Execution ID: {row.enola_id}")
        print(f"Start Date: {row.start_dt}")
        print(f"End Date: {row.end_dt}")
        print(f"Successful: {row.successfull}")
        print(f"Agent ID: {row.agent_id}")
        print(f"Agent Name: {row.agent_name}")
        print(f"User ID: {row.user_id}")
        print(f"Session ID: {row.session_id}")
        print(f"Channel: {row.channel}")
        print(f"Message Input: {row.message_input}")
        print(f"Message Output: {row.message_output}")
        print("-------------------------------------")
```

## Explanation

This code demonstrates how to extract execution data from the Enola-AI platform:

1. **Create a `GetExecutions` Instance**: Initialize an instance of `GetExecutions` with your token.

2. **Define Query Parameters**: Use the `query` method to specify the date range, limit, and filters for the executions you want to retrieve.

   - **Date Range**: Retrieve executions from `"2024-10-28T00:00"` to `"2024-12-31T23:59"`.
   - **Limit**: Set a limit of 20 executions per page.
   - **Evaluation Filter**: Use `eval_id_internal` to exclude executions that have already been internally evaluated.

3. **Retrieve and Process Data**: Use a loop to fetch and process each page of results.

   - **Pagination Control**: The loop continues while there are more pages to retrieve and the page number is less than 70.
   - **Data Check**: Break the loop if no data is returned.
   - **Data Extraction**: For each execution, print out key details such as execution ID, dates, success status, agent information, user ID, session ID, channel, and messages.

## Notes

- **Adjust Query Parameters**: You can modify the `date_from`, `date_to`, `limit`, and other parameters in the `query` method to suit your needs.

- **Data Processing**: The code above prints execution details to the console. You can modify the code to process the data as needed for your application, such as storing it in a database or performing analytics.

- **Pagination**: The loop includes a condition to prevent infinite loops (`exec.get_page_number() < 70`). Adjust this limit based on your expected data volume.

## Summary

By running this code, you can extract execution information from the Enola-AI platform. This allows you to access and utilize your execution data for analysis, processing, reporting, or integrating with other systems.

---
## 6. Documentation

In this section, you can find documentation about Enola-AI, including step-by-step instructions and examples, along with explanations of the system's functionalities:

For the complete documentation, you can visit our [User Guide](docs/user_guide.md) covering the following sections:
- 6.1. Sending Online Chat Data
- 6.2. Sending Online Score Data
- 6.3. Sending Multiple Tasks
- 6.4. Sending File Information
- 6.5. Sending API Information
- 6.6. Sending Cost Information
- 6.7. Sending Batch Score Data

---

## 7. Summary

- This documentation provides a basic guide on using the Enola-AI Python library to initialize tracking and send data.
- For more detailed documentation, you can visit our [User Guide](docs/user_guide.md).
- For a comprehensive guide on how to build a Chatbot using Ollama, you can visit our section [Build a Chatbot using Ollama](docs/building_chatbot_ollama.md).
- If you have questions, you are invited to visit our [Frequently Asked Questions](docs/faq.md) section.


---

## 8. Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or corrections.

When contributing, please ensure to:

- Follow the existing coding style.
- Write clear commit messages.
- Update documentation as necessary.
- Ensure that any code changes are covered by tests.

---

## 9. License

This project is licensed under the **BSD 3-Clause License**.

---

## 10. Contact

For any inquiries or support, please contact us at [help@huemulsolutions.com](mailto:help@huemulsolutions.com).
