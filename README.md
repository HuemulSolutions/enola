# Enola-AI Python Library Documentation

## Table of Contents

1. [Introduction](#1-introduction)
2. [Features](#2-features)
3. [Requirements](#3-requirements)
4. [Installation](#4-installation)
5. [Getting Started](#5-getting-started)
   - 5.1. [Initializing Tracking](#51-initializing-tracking)
   - 5.2. [Understanding Steps in Enola-AI](#52-understanding-steps-in-enola-ai)
     - [Generic Steps](#generic-steps)
     - [LLM Steps](#llm-steps)
6. [Documentation](#6-documentation)
   - 6.1. [Sending Online Chat Data](#61-sending-online-chat-data)
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

### 5.2. Understanding Steps in Enola-AI

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

In this section, you will find basic documentation about Enola-AI, including step-by-step instructions and examples, along with explanations of the system's functionalities:

This initial guide is covering the following section:
- 6.1. Sending Online Chat Data

For the rest of the documentation, you can visit our [User Guide](docs/user_guide.md) covering all of the sections:
- 6.2. Sending Online Score Data
- 6.3. Sending Multiple Tasks
- 6.4. Sending File Information
- 6.5. Sending API Information
- 6.6. Sending Cost Information
- 6.7. Sending Batch Score Data

For a comprehensive guide on how to build a Chatbot using Ollama, you can visit our section [Build a Chatbot using Ollama](docs/building_chatbot_ollama.md).

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
   step_chat = monitor.new_step("User Inquiry")
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
step_chat = monitor.new_step("User Inquiry")

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
   - For a comprehensive guide on how to build a Chatbot using Ollama, you can visit our section [Build a Chatbot using Ollama](docs/building_chatbot_ollama.md).
  

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

## 7. Summary

- This documentation provides a basic guide on using the Enola-AI Python library to initialize tracking and send online chat data.
- For more detailed documentation, you can visit our [User Guide](docs/user_guide.md).
- For a comprehensive guide on how to build a Chatbot using Ollama, you can visit our section [Build a Chatbot using Ollama](docs/building_chatbot_ollama.md)


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
