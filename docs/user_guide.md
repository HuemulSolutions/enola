# Enola-AI Python Library Documentation

## Table of Contents

1. [Introduction](#1-introduction)
2. [Features](#2-features)
3. [Requirements](#3-requirements)
4. [Installation](#4-installation)
5. [Getting Started](#5-getting-started)
   - 5.1. [Initializing Tracking](#51-initializing-tracking)
   - 5.2. [Example: Basic Tracking Initialization](#52-example-basic-tracking-initialization)
   - 5.3. [Understanding Steps in Enola-AI](#53-understanding-steps-in-enola-ai)
     - [Generic Steps](#generic-steps)
     - [LLM Steps](#llm-steps)
6. [Documentation](#6-documentation)
   - 6.1. [Sending Online Chat Data](#61-sending-online-chat-data)
   - 6.2. [Sending Online Score Data](#62-sending-online-score-data)
   - 6.3. [Sending Multiple Tasks](#63-sending-multiple-tasks)
   - 6.4. [Sending File Information](#64-sending-file-information)
   - 6.5. [Sending API Information](#65-sending-api-information)
   - 6.6. [Sending Cost Information](#66-sending-cost-information)
   - 6.7. [Sending Batch Score Information](#67-sending-batch-score-information)
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

### Initializing Tracking
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

#### **3. Initialize the Tracking Agent**
Load the Enola API token and initialize the tracking agent.
```python
# Load .env file
load_dotenv()
# Set up your token
token = os.getenv('ENOLA_TOKEN')

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
    message_input="Hello, what can you do?"  # Input message from the user
)
```
---

#### **Complete Example: Basic Tracking Initialization**

```python
# Import necessary libraries
from enola.tracking import Tracking
from dotenv import load_dotenv
import os

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
    message_input="Hello, what can you do?"  # Input message from the user
)
```
After executing the script and initializing the tracking agent, you should get a console output like this:
```plaintext
2024-10-17 09:31:23,539 WELCOME to Enola...
2024-10-17 09:31:23,540 authorized...
2024-10-17 09:31:23,540 STARTED!!!
```
**This means you have successfully connected to Enola and initialized your first tracking agent!**

### Understanding Steps in Enola-AI

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

---
## 6. Documentation

In this section, you will find complete documentation about Enola-AI, including step-by-step instructions and examples, along with explanations of the system's functionalities:

Currently, this guide is covering the following sections:
- 6.1. Sending Online Chat Data
- 6.2. Sending Online Score Data (available soon)
- 6.3. Sending Multiple Tasks
- 6.4. Sending File Information
- 6.5. Sending API Information
- 6.6. Sending Cost Information
- 6.7. Sending Batch Score Data (available soon)
- Guide: Creating a Chatbot using Ollama (available soon)

---
### 6.1. Sending Online Chat Data

When working with conversational AI agents, it's essential to track user interactions and model responses. Enola-AI allows you to send online chat data for monitoring and evaluation.

#### **Understanding Online Chat Data Tracking**

- **Purpose**: Tracking online chat data helps in monitoring user interactions, evaluating model performance, and ensuring compliance with regulatory standards.
  
- **Benefits**:
  - **Performance Monitoring**: Analyze how the model responds to user inputs.
  - **User Experience**: Improve the conversational flow by understanding user queries and model answers.
  - **Compliance**: Keep records of interactions for auditing and compliance purposes.

#### **Step-by-Step Guide**

1. **Import Necessary Libraries**

   ```python
   # Import necessary libraries
   from enola.tracking import Tracking
   from dotenv import load_dotenv
   import os
   ```

2. **Load the Enola API Token**

   ```python
   # Load .env file and set up your token
   load_dotenv()
   token = os.getenv('ENOLA_TOKEN')
   ```

3. **Define User Input**

   ```python
   # Define the user's message input
   user_input = "What car offers good performance for a low cost?"
   ```

4. **Initialize the Tracking Object**

   Create an instance of the `Tracking` class to start tracking the execution.

   ```python
   monitor = Tracking(
       token=token,
       name="Chat Session Tracking V1",
       is_test=True,  # Set to False in production
       app_id="ChatApp",
       user_id="User123",
       session_id="Session456",
       channel_id="console",
       ip="192.168.1.1",
       message_input=user_input
   )
   ```

5. **Create a New LLM Step**

   Since we're interacting with a language model, create an LLM step.

   ```python
   # Create an LLM Step
   step_chat = monitor.new_llm_step("User Inquiry")
   ```

6. **Add Extra Information**

   Add any additional information relevant to the step, such as the user's question.

   ```python
   # Add user's question
   step_chat.add_extra_info("UserQuestion", user_input)
   ```

7. **Process the User Input with the Language Model**

   Simulate the model generating a response to the user's question.

   ```python
   # Simulated model response
   model_response = "The Honda Civic offers great performance at a reasonable price."
   ```

   > **Note**: Replace the simulated response with actual model integration in a production environment.

8. **Add the Model's Response to the Step**

   ```python
   # Add model's response
   step_chat.add_extra_info("ModelResponse", model_response)
   ```

9. **Close the LLM Step**

   Indicate that the step has completed successfully, and include token usage and costs.

   ```python
   # Close the LLM Step
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

   > **Note**: Replace token counts and costs with actual values from your language model provider.

10. **Execute the Tracking**

    Send the tracking data to the Enola-AI server.

    ```python
    # Execute the tracking and send the data to Enola-AI server
    monitor.execute(
        successfull=True,
        message_output=model_response,
        num_iteratons=1
    )
    ```

#### **Complete Example: Sending Online Chat Data**

```python
# Import necessary libraries
from enola.tracking import Tracking
from dotenv import load_dotenv
import os

# Load .env file and set up your token
load_dotenv()
token = os.getenv('ENOLA_TOKEN')

# Define the user's message input
user_input = "What car offers good performance for a low cost?"

# Initialize the tracking object
monitor = Tracking(
    token=token,
    name="Chat Session Tracking V1",
    is_test=True,
    app_id="ChatApp",
    user_id="User123",
    session_id="Session456",
    channel_id="console",
    ip="192.168.1.1",
    message_input=user_input
)

# Create an LLM Step
step_chat = monitor.new_llm_step("User Inquiry")

# Add user's question
step_chat.add_extra_info("UserQuestion", user_input)

# Simulated model response
model_response = "The Honda Civic offers great performance at a reasonable price."

# Add model's response
step_chat.add_extra_info("ModelResponse", model_response)

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

#### **Explanation**

- **Initialization**:
  - Imported necessary libraries and loaded the Enola API token.
  - Initialized the `Tracking` object with user and session information.

- **Creating the LLM Step**:
  - Created a new LLM step to track the language model interaction.
  - Added the user's question and the model's response as extra information.

- **Closing the Step**:
  - Closed the LLM step, including token usage and cost information.
  - Executed the tracking to send data to Enola-AI.

#### **Notes**

- **Model Integration**:
  - Ensure that you capture actual token counts and costs.
  - Ensure that you add your actual model response, since the message in the variable `model_response` is being simulated in this example:
    ```Python
    # Simulated model response
    model_response = "The Honda Civic offers great performance at a reasonable price."
    ```
   - However, when your Python script effectively connects with an LLM Model (eg. Ollama running on your local machine or through an API), you can expect a real response that will be stored in the variable `model_response`.
   - You can check our user guide to create a simple chatbot using Ollama running on your local machine (available soon).
  

- **Error Handling**:
  - Implement error handling for cases where the language model fails or returns an unexpected response.
  - Use `step_chat.add_error()` to log any errors.
  
- **Executing the Tracking**:
  - After executing the script and the tracking, you should see a console output indicating that you have successfully sent the data to the Enola-AI servers, allowing you to monitor and evaluate your data on the Enola-AI platform:

	```console
	Chat Session Tracking V1: sending to server... 
	Chat Session Tracking V1: finish OK! 
	```

---
### 6.2. Sending Online Score Data (available soon)
---

### 6.3. Sending Multiple Tasks

When your agent performs multiple tasks or steps within a single execution, you can track each step individually using Enola-AI. This is crucial for detailed monitoring, debugging, and ensuring that each part of your application functions correctly.

In this example, we'll demonstrate how to:

- Validate the user's input.
- Invoke a Language Model (LLM) to process the input.
- Validate the model's response.
- Execute tracking for all steps.

### Step-by-Step Guide

#### 1. **Initialize the Tracking Object**

First, import the necessary modules and initialize the `Tracking` object. Use a unique `session_id` to identify the session.
In this example, a chatbot made with Ollama will be used for model responses. You can use your own model or
You can visit our Guide: Creating a Chatbot using Ollama (available soon).

```python
from enola.tracking import Tracking
from enola.enola_types import ErrOrWarnKind
from enola_ollama_v2 import ollama_chat  # Import your model/chatbot function
from dotenv import load_dotenv
import os
import uuid

# Load .env file
load_dotenv()

# Set up your token
token = os.getenv('ENOLA_TOKEN')

# Generate a session_id
session_id = str(uuid.uuid4())

# User input
user_input = "What is an LLM?"

monitor = Tracking(
    token=token,
    name="Multi-Step Execution",
    is_test=True,
    app_id="MultiTaskApp",
    user_id="User456",
    session_id=session_id,
    channel_id="console",
    ip="127.0.0.1",
    message_input=user_input
)
```

#### 2. **Step 1: Validate User Input (Generic Step)**

Create a new step to validate the user's input. If the input is empty, log an error and execute the tracking with `successfull=False`. Otherwise, close the step as successful.

```python
# Step 1: Validate User Input (Generic Step)
step1 = monitor.new_step("Validate User Input")
if not user_input.strip():
    step1.add_error(
        id="EmptyUserInput",
        message="User input is empty.",
        kind=ErrOrWarnKind.EXTERNAL
    )
    monitor.close_step_others(
        step=step1,
        successfull=False,
        message_output="User input is empty."
    )
    # Execute tracking with successfull=False
    monitor.execute(
        successfull=False,
        message_output="User input is empty.",
        num_iteratons=1
    )
    print("User input is empty.")
    exit()  # Exit or handle accordingly
else:
    monitor.close_step_others(
        step=step1,
        successfull=True,
        message_output="User input is valid."
    )
```

#### 3. **Step 2: Invoke LLM (LLM Step)**

Invoke the language model to process the user's input. Wrap the invocation in a `try/except` block to catch any exceptions. Log the model's response, token usage, and costs.

```python
# Step 2: Invoke LLM (LLM Step)
step2 = monitor.new_step("Invoke LLM")
try:
    # Call the model/chatbot to get the response
    response = ollama_chat(user_input)  # The response from the external model/chatbot
    step2.add_extra_info("ModelResponse", response)
    token_input_num = len(user_input.split())
    token_output_num = len(response.split())
    token_total_cost = 0.01  # Example cost
    monitor.close_step_token(
        step=step2,
        successfull=True,
        message_output="LLM processing completed.",
        token_input_num=token_input_num,
        token_output_num=token_output_num,
        token_total_cost=token_total_cost,
        token_input_cost=0.005,
        token_output_cost=0.005
    )
except Exception as e:
    # Handle exception if LLM invocation fails
    step2.add_error(
        id="LLMInvocationError",
        message=str(e),
        kind=ErrOrWarnKind.EXTERNAL
    )
    monitor.close_step_token(
        step=step2,
        successfull=False,
        message_output="Error occurred during LLM invocation."
    )
    # Execute tracking with successfull=False
    monitor.execute(
        successfull=False,
        message_output="Error occurred during LLM invocation.",
        num_iteratons=2
    )
    print("An error occurred while processing your request.")
    exit()  # Exit or handle accordingly
```

#### 4. **Step 3: Validate Model Response (Generic Step)**

Create a step to validate the model's response. If the response is empty, log an error and execute the tracking with `successfull=False`. Otherwise, close the step as successful.

```python
# Step 3: Validate Model Response (Generic Step)
step3 = monitor.new_step("Validate Model Response")
if not response.strip():
    step3.add_error(
        id="EmptyModelResponse",
        message="Model response is empty.",
        kind=ErrOrWarnKind.EXTERNAL
    )
    monitor.close_step_others(
        step=step3,
        successfull=False,
        message_output="Model response is empty."
    )
    # Execute tracking with successfull=False
    monitor.execute(
        successfull=False,
        message_output="Model response is empty.",
        num_iteratons=3
    )
    print("Model response is empty.")
    exit()  # Exit or handle accordingly
else:
    monitor.close_step_others(
        step=step3,
        successfull=True,
        message_output="Model response is valid."
    )
```

#### 5. **Execute the Tracking**

After all steps are completed successfully, execute the tracking with `successfull=True` and proceed with your application logic.

```python
# Execute the tracking
monitor.execute(
    successfull=True,
    message_output=response,
    num_iteratons=3
)

# Continue with the rest of your application logic
print(f"Response: {response}")
```

### Complete Example

Here's the full code combined:

```python
from enola.tracking import Tracking
from enola.enola_types import ErrOrWarnKind
from enola_ollama_v2 import ollama_chat  # Import your model/chatbot function
from dotenv import load_dotenv
import os
import uuid

# Load .env file
load_dotenv()

# Set up your token
token = os.getenv('ENOLA_TOKEN')

# Generate a session_id
session_id = str(uuid.uuid4())

# User input
user_input = "What is an LLM?"

monitor = Tracking(
    token=token,
    name="Multi-Step Execution",
    is_test=True,
    app_id="MultiTaskApp",
    user_id="User456",
    session_id=session_id,
    channel_id="console",
    ip="127.0.0.1",
    message_input=user_input
)

# Step 1: Validate User Input (Generic Step)
step1 = monitor.new_step("Validate User Input")
if not user_input.strip():
    step1.add_error(
        id="EmptyUserInput",
        message="User input is empty.",
        kind=ErrOrWarnKind.EXTERNAL
    )
    monitor.close_step_others(
        step=step1,
        successfull=False,
        message_output="User input is empty."
    )
    # Execute tracking with successfull=False
    monitor.execute(
        successfull=False,
        message_output="User input is empty.",
        num_iteratons=1
    )
    print("User input is empty.")
    exit()  # Exit or handle accordingly
else:
    monitor.close_step_others(
        step=step1,
        successfull=True,
        message_output="User input is valid."
    )

# Step 2: Invoke LLM (LLM Step)
step2 = monitor.new_step("Invoke LLM")
try:
    # Call the model/chatbot to get the response
    response = ollama_chat(user_input)  # The response from the external model/chatbot
    step2.add_extra_info("ModelResponse", response)
    token_input_num = len(user_input.split())
    token_output_num = len(response.split())
    token_total_cost = 0.01  # Example cost
    monitor.close_step_token(
        step=step2,
        successfull=True,
        message_output="LLM processing completed.",
        token_input_num=token_input_num,
        token_output_num=token_output_num,
        token_total_cost=token_total_cost,
        token_input_cost=0.005,
        token_output_cost=0.005
    )
except Exception as e:
    # Handle exception if LLM invocation fails
    step2.add_error(
        id="LLMInvocationError",
        message=str(e),
        kind=ErrOrWarnKind.EXTERNAL
    )
    monitor.close_step_token(
        step=step2,
        successfull=False,
        message_output="Error occurred during LLM invocation."
    )
    # Execute tracking with successfull=False
    monitor.execute(
        successfull=False,
        message_output="Error occurred during LLM invocation.",
        num_iteratons=2
    )
    print("An error occurred while processing your request.")
    exit()  # Exit or handle accordingly

# Step 3: Validate Model Response (Generic Step)
step3 = monitor.new_step("Validate Model Response")
if not response.strip():
    step3.add_error(
        id="EmptyModelResponse",
        message="Model response is empty.",
        kind=ErrOrWarnKind.EXTERNAL
    )
    monitor.close_step_others(
        step=step3,
        successfull=False,
        message_output="Model response is empty."
    )
    # Execute tracking with successfull=False
    monitor.execute(
        successfull=False,
        message_output="Model response is empty.",
        num_iteratons=3
    )
    print("Model response is empty.")
    exit()  # Exit or handle accordingly
else:
    monitor.close_step_others(
        step=step3,
        successfull=True,
        message_output="Model response is valid."
    )

# Execute the tracking
monitor.execute(
    successfull=True,
    message_output=response,
    num_iteratons=3
)

# Continue with the rest of your application logic
print(f"Response: {response}")
```

### Explanation

- **Session Initialization**: A unique `session_id` is generated for tracking purposes.
- **User Input**: The user's question is stored in `user_input`.
- **Step 1: Validate User Input**:
  - Checks if the input is empty.
  - Logs an error and exits if invalid.
- **Step 2: Invoke LLM**:
  - Calls the `ollama_chat` function to get the model's response.
  - Catches exceptions during the model invocation.
  - Logs token usage and costs.
- **Step 3: Validate Model Response**:
  - Checks if the model's response is empty.
  - Logs an error and exits if invalid.
- **Execute Tracking**:
  - After all steps are successfully completed, tracking data is sent to Enola-AI.
- **Application Logic**:
  - Prints the model's response to the user.

### Notes

- **Error Handling**:
  - Each critical step includes error handling to ensure that failures are properly logged and tracked.
- **Cost Estimation**:
  - Token costs are estimated; replace with actual calculations if available.
- **Dynamic Values**:
  - Replace placeholders like `User456` with dynamic user identifiers if applicable.


### Benefits of Tracking Multiple Tasks

- **Granular Monitoring**: By tracking each step, you gain insights into where issues may occur.
- **Debugging**: Easier to pinpoint failures and understand their context.
- **Compliance**: Detailed logs can help with regulatory compliance and auditing.
- **Optimization**: Identify bottlenecks or inefficiencies in your application.

By incorporating multiple steps and thorough validation, you enhance the reliability and transparency of your application. Enola-AI provides the tools necessary to implement this level of detailed tracking.

---

### 6.4. Sending File Information

When your application processes files (e.g., images, documents), you can use Enola-AI to log file-related information for tracking purposes. This is particularly useful for monitoring file processing steps, debugging, and maintaining an audit trail.

### Step-by-Step Guide

#### 1. **Initialize the Tracking Object**

First, import the necessary modules and initialize the `Tracking` object. Generate a unique `session_id` to identify the session.

```python
from enola.tracking import Tracking
from enola_ollama_v2 import ollama_chat  # Import your model/chatbot if applicable
from dotenv import load_dotenv
import os
import uuid

# Load .env file
load_dotenv()

# Set up your token
token = os.getenv('ENOLA_TOKEN')

# Generate a session_id
session_id = str(uuid.uuid4())

# User input
user_input = "User File information"

monitor = Tracking(
    token=token,
    name="Sending File Information",
    is_test=True,  # Set to False in production
    app_id="SendingFileInformationApp",
    user_id="User456",
    session_id=session_id,
    channel_id="console",
    ip="127.0.0.1",
    message_input=user_input
)
```

#### 2. **Create a New Step for File Processing**

Create a new step in the tracking session to represent the file processing task.

```python
# Step: File Processing
step_file = monitor.new_step("File Processing")
```

#### 3. **Add File Information**

Use the `add_file_link` method to include details about the file being processed.

```python
# Add file information
step_file.add_file_link(
    name="UserDocument",
    url="https://example.com/documents/userdoc.pdf",
    type="pdf",
    size_kb=2048,
    description="User uploaded document."
)
```

- **Parameters:**
  - `name`: Name of the file.
  - `url`: URL where the file is located.
  - `type`: File type or extension.
  - `size_kb`: Size of the file in kilobytes.
  - `description`: A brief description of the file.

#### 4. **Close the File Processing Step**

After processing the file, close the step using the `close_step_doc` method, including any relevant metrics.

```python
# Close the file processing step
monitor.close_step_doc(
    step=step_file,
    successfull=True,
    message_output="File processed successfully.",
    doc_num=1,
    doc_pages=10,
    doc_size=2048,
    doc_char=5000,
    doc_cost=0.005
)
```

- **Parameters:**
  - `step`: The step object representing the file processing.
  - `successfull`: Boolean indicating whether the step was successful.
  - `message_output`: A message summarizing the outcome.
  - `doc_num`: Number of documents processed.
  - `doc_pages`: Total number of pages in the document(s).
  - `doc_size`: Total size in kilobytes.
  - `doc_char`: Number of characters processed.
  - `doc_cost`: Cost associated with processing the document(s).

#### 5. **Execute the Tracking**

Finally, call the `execute` method to send the tracking data to the Enola-AI servers.

```python
# Execute call to send data to the server
monitor.execute(
    successfull=True,
    message_output="File processed successfully.",
    num_iteratons=1  # Number of steps or iterations
)
```

- **Parameters:**
  - `successfull`: Boolean indicating whether the overall execution was successful.
  - `message_output`: A message summarizing the overall outcome.
  - `num_iteratons`: The number of steps or iterations in this execution.

### Complete Example

Below is the complete code that demonstrates how to send file information using Enola-AI:

```python
from enola.tracking import Tracking
from enola_ollama_v2 import ollama_chat  # Import your model/chatbot if applicable
from dotenv import load_dotenv
import os
import uuid

# Load .env file
load_dotenv()

# Set up your token
token = os.getenv('ENOLA_TOKEN')

# Generate a session_id
session_id = str(uuid.uuid4())

# User input
user_input = "User File information"

monitor = Tracking(
    token=token,
    name="Sending File Information",
    is_test=True,
    app_id="SendingFileInformationApp",
    user_id="User456",
    session_id=session_id,
    channel_id="console",
    ip="127.0.0.1",
    message_input=user_input
)

# Step: File Processing
step_file = monitor.new_step("File Processing")

# Add file information
step_file.add_file_link(
    name="UserDocument",
    url="https://example.com/documents/userdoc.pdf",
    type="pdf",
    size_kb=2048,
    description="User uploaded document."
)

# Close the file processing step
monitor.close_step_doc(
    step=step_file,
    successfull=True,
    message_output="File processed successfully.",
    doc_num=1,
    doc_pages=10,
    doc_size=2048,
    doc_char=5000,
    doc_cost=0.005
)

# Execute call to send data to the server
monitor.execute(
    successfull=True,
    message_output="File processed successfully.",
    num_iteratons=1  # Number of steps or iterations
)
```

### Explanation

- **Initialization:**
  - We import necessary modules and set up the Enola-AI tracking object.
  - `session_id` is generated to uniquely identify the session.
  - `user_input` represents the user's input message.

- **File Processing Step:**
  - A new step named "File Processing" is created.
  - File details are added using `add_file_link`.
  - The step is closed using `close_step_doc`, including relevant metrics.

- **Executing Tracking:**
  - The `monitor.execute` method is called to send all collected tracking data to Enola-AI servers.

### Notes

- **Error Handling:**
  - You may wish to add error handling around the file processing and tracking execution to catch any exceptions.

- **Token Validation:**
  - Ensure that the `ENOLA_TOKEN` environment variable is correctly set and that the token is valid.

### Benefits of Tracking File Information

- **Monitoring:**
  - Keep track of all files processed by your application.

- **Debugging:**
  - Easily identify and troubleshoot issues related to file processing.

- **Auditing:**
  - Maintain an audit trail of file interactions for compliance and reporting purposes.

- **Resource Management:**
  - Monitor costs associated with file processing to optimize resource usage.

By following this guide, you can effectively use Enola-AI to track file processing activities within your application.

---

### 6.5. Sending API Information

When your application makes API calls, you can use Enola-AI to log detailed information about these interactions. This is useful for monitoring API usage, debugging issues, and maintaining an audit trail of external service integrations.

### Understanding API Information Tracking

Tracking API information involves recording details such as:

- **API Name**: A descriptive name for the API.
- **Method**: The HTTP method used (e.g., `GET`, `POST`).
- **URL**: The endpoint URL of the API.
- **Request Headers and Body**: The headers and body sent with the request.
- **Response Payload**: The data received from the API.
- **Description**: A brief description of the API call's purpose.

### Step-by-Step Guide

#### 1. **Initialize the Tracking Object**

First, import the necessary modules and initialize the `Tracking` object. Generate a unique `session_id` to identify the session.

```python
from enola.tracking import Tracking
from dotenv import load_dotenv
import os
import uuid

# Load .env
load_dotenv()

# Set up your Enola-AI token
token = os.getenv('ENOLA_TOKEN')

# Generate a unique session_id
session_id = str(uuid.uuid4())

# User input
user_input = "Requesting data from Sample API"

monitor = Tracking(
    token=token,
    name="Sending API Information",
    is_test=True,
    app_id="SampleAPIApp",
    user_id="User123",
    session_id=session_id,
    channel_id="console",
    ip="127.0.0.1",
    message_input=user_input
)
```

#### 2. **Create a New Step for the API Call**

Create a new step in the tracking session to represent the API call.

```python
# Step: External API Call
step_api = monitor.new_step("External API Call")
```

#### 3. **Simulate an API Call**

For this example, we'll simulate an API call to avoid complexities with real APIs and potential issues with large JSON payloads.

```python
# Simulated API request and response data
api_name = "Sample API"
method = "GET"
url = "https://api.sample.com/data"
headers_sent = {"Authorization": "Bearer sample_token"}
body_sent = ""  # Empty for GET requests
payload_received = '{"status": "success", "data": {"id": 1, "value": "Sample Data"}}'
description = "Fetching sample data from Sample API."
```

#### 4. **Add API Data to the Step**

Use the `add_api_data` method to include details about the API call.

```python
import json

# Convert headers and payload to JSON strings
headerToSend = json.dumps(headers_sent)
# payload_received is already a JSON string in this case

# Add API data to the step
step_api.add_api_data(
    name=api_name,
    method=method,
    url=url,
    bodyToSend=body_sent,
    headerToSend=headerToSend,
    payloadReceived=payload_received,
    description=description
)
```

#### 5. **Close the API Step**

After adding the API data, close the step using `close_step_others`.

```python
# Close the API step
monitor.close_step_others(
    step=step_api,
    successfull=True,
    message_output="API call simulated successfully.",
    others_cost=0.001  # Include cost if applicable
)
```

#### 6. **Execute the Tracking**

Finally, execute the tracking to send the data to Enola-AI.

```python
# Execute the tracking
monitor.execute(
    successfull=True,
    message_output="API information processed successfully.",
    num_iteratons=1  # Number of steps or iterations
)
```

### Complete Example

Here's the full code combined:

```python
from enola.tracking import Tracking
from dotenv import load_dotenv
import os
import uuid
import json

# Load .env
load_dotenv()

# Set up your Enola-AI token
token = os.getenv('ENOLA_TOKEN')

# Generate a unique session_id
session_id = str(uuid.uuid4())

# User input
user_input = "Requesting data from Sample API"

monitor = Tracking(
    token=token,
    name="Sending API Information",
    is_test=True,
    app_id="SampleAPIApp",
    user_id="User123",
    session_id=session_id,
    channel_id="console",
    ip="127.0.0.1",
    message_input=user_input
)

# Step: External API Call
step_api = monitor.new_step("External API Call")

# Simulated API request and response data
api_name = "Sample API"
method = "GET"
url = "https://api.sample.com/data"
headers_sent = {"Authorization": "Bearer sample_token"}
body_sent = ""  # Empty for GET requests
payload_received = '{"status": "success", "data": {"id": 1, "value": "Sample Data"}}'
description = "Fetching sample data from Sample API."

# Convert headers to JSON string
headerToSend = json.dumps(headers_sent)
# payload_received is already a JSON string

# Add API data to the step
step_api.add_api_data(
    name=api_name,
    method=method,
    url=url,
    bodyToSend=body_sent,
    headerToSend=headerToSend,
    payloadReceived=payload_received,
    description=description
)

# Close the API step
monitor.close_step_others(
    step=step_api,
    successfull=True,
    message_output="API call simulated successfully.",
    others_cost=0.001  # Include cost if applicable
)

# Execute the tracking
monitor.execute(
    successfull=True,
    message_output="API information processed successfully.",
    num_iteratons=1  # Number of steps or iterations
)
```

### Explanation

- **Initialization**:
  - We import necessary modules and set up the Enola-AI tracking object.
  - `session_id` is generated to uniquely identify the session.
  - `user_input` represents the user's input message.

- **Simulated API Call**:
  - Since we're simulating, we manually define the API call details.
  - This avoids issues with real APIs and ensures the example is straightforward.

- **Adding API Data**:
  - The `add_api_data` method is used to add the API call information to the step.
  - `headerToSend` and `payloadReceived` are provided as valid JSON strings.

- **Closing the Step**:
  - The API step is closed using `close_step_others`, including any associated costs.

- **Executing Tracking**:
  - The `monitor.execute` method sends all collected tracking data to Enola-AI servers.

### Notes

- **Valid JSON Strings**:
  - Ensure that `headerToSend` and `payloadReceived` are valid JSON strings.
  - Use `json.dumps` to serialize dictionaries to JSON.

- **Error Handling**:
  - In a real-world scenario, include error handling for API calls and log any exceptions using `step_api.add_error`.

- **Sensitive Data**:
  - Be cautious when including sensitive data such as API keys or tokens in headers or payloads.
  - Consider anonymizing or omitting such data.

### Benefits of Tracking API Information

- **Monitoring**:
  - Keep track of external API usage and performance.

- **Debugging**:
  - Simplify troubleshooting by logging request and response details.

- **Auditing**:
  - Maintain an audit trail of interactions with external services.

- **Cost Management**:
  - Monitor costs associated with API usage.



By following this guide, you can effectively log API call information in your application using Enola-AI, even when simulating API interactions. This helps in maintaining transparency and facilitates better monitoring and debugging of your application.

---

### 6.6. Sending Cost Information

Tracking the cost associated with different steps in your application helps monitor resource usage and optimize performance. Enola-AI allows you to include cost information when closing steps, providing detailed insights into the expenses incurred during execution.

### Understanding Cost Tracking

Costs can be associated with various types of steps, such as:

- **LLM Steps**: Costs related to token usage in language models.
- **API Calls**: Expenses from external API calls.
- **Image Processing**: Costs for processing images, including the number of images, their sizes, and total cost.

Including cost information helps in:

- **Budget Monitoring**: Keep track of expenses to stay within budget.
- **Resource Optimization**: Identify costly operations and optimize them.
- **Transparency**: Provide clear insights into the operational costs for stakeholders.

### Step-by-Step Guide

#### 1. **Including Cost Information When Closing Steps**

When you close a step, you can include cost-related parameters relevant to the type of step. Below are examples for different types of steps.

#### 2. **For LLM Steps**

After performing operations involving a language model, you can close the LLM step and include token costs.

```python
# Step: Invoke LLM
step_llm = monitor.new_step("Invoke LLM")

# Call the language model to get a response
response = ollama_chat(user_input)

# Estimate token counts (replace with actual counts if available)
token_input_num = len(user_input.split())
token_output_num = len(response.split())

# Calculate token costs (replace with actual cost calculations)
token_input_cost = token_input_num * 0.0001  # Example cost per token
token_output_cost = token_output_num * 0.0001
token_total_cost = token_input_cost + token_output_cost

# Close LLM step with token costs
monitor.close_step_token(
    step=step_llm,
    successfull=True,
    message_output="LLM processing completed.",
    token_input_num=token_input_num,
    token_output_num=token_output_num,
    token_input_cost=token_input_cost,
    token_output_cost=token_output_cost,
    token_total_cost=token_total_cost
)
```


#### 3. **For Image Processing**

When processing images, include costs such as the number of images, total size, and processing cost.

```python
# Step: Image Processing
step_image = monitor.new_step("Image Processing")

# Process images
# ... (your image processing logic here) ...

# Example image processing metrics
image_num = 2
image_size = 4096  # Total size in KB
image_cost = 0.01  # Example cost

# Close image processing step with cost information
monitor.close_step_image(
    step=step_image,
    successfull=True,
    message_output="Image processed.",
    image_num=image_num,
    image_size=image_size,
    image_cost=image_cost
)
```

#### 4. **For API Calls**

After making an API call, you can include the associated cost when closing the step.

```python
# Step: External API Call
step_api = monitor.new_step("External API Call")

# Perform the API call
# ... (your API call logic here) ...

# Assume we have a variable 'api_cost' representing the cost of the API call
api_cost = 0.002  # Example cost

# Close API step with cost information
monitor.close_step_others(
    step=step_api,
    successfull=True,
    message_output="API call successful.",
    others_cost=api_cost
)
```

### Complete Example

Here's a comprehensive example demonstrating how to include cost information when closing steps in a tracking session.

```python
from enola.tracking import Tracking
from enola_ollama_v2 import ollama_chat  # Import your model/chatbot
from dotenv import load_dotenv
import os
import uuid

# Load .env file
load_dotenv()

# Set up your token
token = os.getenv('ENOLA_TOKEN')

# Generate a session_id
session_id = str(uuid.uuid4())

# User input
user_input = "Tell me about planet Earth"

monitor = Tracking(
    token=token,
    name="Cost Tracking Example",
    is_test=True,
    app_id="CostTrackingApp",
    user_id="User789",
    session_id=session_id,
    channel_id="console",
    ip="127.0.0.1",
    message_input="Tell me about planet Earth"
)

# Step 1: Invoke LLM
step_llm = monitor.new_step("Invoke LLM")

# Call the language model to get a response
response = ollama_chat(user_input)

# Estimate token counts (replace with actual counts if available)
token_input_num = len(user_input.split())
token_output_num = len(response.split())

# Calculate token costs (replace with actual cost calculations)
token_input_cost = token_input_num * 0.0001  # Example cost per token
token_output_cost = token_output_num * 0.0001
token_total_cost = token_input_cost + token_output_cost

# Close LLM step with token costs
monitor.close_step_token(
    step=step_llm,
    successfull=True,
    message_output=response, # Response from the LLM
    token_input_num=token_input_num,
    token_output_num=token_output_num,
    token_input_cost=token_input_cost,
    token_output_cost=token_output_cost,
    token_total_cost=token_total_cost
)

# Step 2: Image Generation
step_image = monitor.new_step("Image Generation")

# Generate image
# ... (your image generation logic here) ...

# Example image processing metrics
image_num = 1
image_size = 2048  # Size in KB
image_cost = 0.05  # Example cost

# Close image generation step with cost information
monitor.close_step_image(
    step=step_image,
    successfull=True,
    message_output="Image generated successfully.",
    image_num=image_num,
    image_size=image_size,
    image_cost=image_cost
)

# Step 3: External API Call
step_api = monitor.new_step("External API Call")

# Perform the API call
# ... (your API call logic here) ...

# Example API call cost
api_cost = 0.002  # Example cost

# Close the API step with cost information
monitor.close_step_others(
    step=step_api,
    successfull=True,
    message_output="API call successful.",
    others_cost=api_cost
)

# Execute the tracking
monitor.execute(
    successfull=True,
    message_output="Invoke LLM, Image generation, API call completed",
    num_iteratons=3  # Total number of steps
)

# Print question and answer from LLM invoke
print(user_input)
print(response)
```

### Explanation

- **Initialization**:
  - A `Tracking` object is created to monitor the session.
  - A unique `session_id` is generated.
  - The user's input is captured in `user_input`.

- **Step 1: LLM Invocation**:
  - A new step `step_llm` is created for invoking the language model.
  - The LLM is called using `ollama_chat(user_input)`.
  - Token counts and costs are calculated (replace with actual values as needed).
  - The LLM step is closed using `close_step_token`, including cost information.

- **Step 2: Image Generation**:
  - A new step `step_image` is created for image generation.
  - Image generation logic is performed.
  - Image metrics and costs are defined.
  - The image generation step is closed using `close_step_image`, including cost information.
  
- **Step 3: External API Call**:
  - A new step `step_api` is created for API call.
  - API call logic is performed.
  - API cost is defined.
  - The API call step is closed using `close_step_others`, including cost information.

- **Executing Tracking**:
  - The `monitor.execute()` method is called to send the tracking data to Enola-AI.

### Notes

- **Actual Cost Calculations**:
  - Replace example cost calculations with actual values based on your application's pricing model or resource consumption.

- **Error Handling**:
  - Include error handling to manage exceptions and record unsuccessful steps if necessary.

- **Token Counts**:
  - Use actual token counts from your language model provider for accurate cost tracking.


### Benefits of Tracking Costs

- **Budget Management**: Helps in keeping track of expenses and ensuring they align with budgetary constraints.
- **Performance Optimization**: Identifies costly operations, allowing you to optimize or refactor them.
- **Transparency**: Provides stakeholders with clear insights into where resources are being utilized.
- **Scalability Planning**: Assists in forecasting costs when scaling the application.

By incorporating cost information into your tracking steps, you gain valuable insights into the operational expenses of your application. This enables better decision-making regarding resource allocation, budgeting, and optimizing performance.

---

### 6.7. Sending Batch Score Information (available soon)

---

## 7. Summary

This documentation provides a guide on using the Enola-AI Python library to initialize tracking and explains the following sections:
- 6.1. Sending Online Chat Data
- 6.3. Sending Multiple Tasks
- 6.4. Sending File Information
- 6.5. Sending API Information
- 6.6. Sending Cost Information

Future sections will cover additional features as they become available:
- 6.2 Sending Online Score Data
- 6.7 Sending Batch Score Information
- Guide: Creating a Chatbot using Ollama

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
