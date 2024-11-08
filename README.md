# Enola-AI Python Library Documentation

## Table of Contents

1. [Introduction](#1-introduction)
2. [Features](#2-features)
3. [Requirements](#3-requirements)
4. [Installation](#4-installation)
5. [Getting Started](#5-getting-started)
   - 5.1. [Initializing Tracking](#51-initializing-tracking)
   - 5.2. [Complete Example: Basic Tracking Initialization](#52-complete-example-basic-tracking-initialization)
6. [Understanding how Enola-AI works](#6-understanding-how-enola-ai-works)
   - 6.1. [Track Activity](#61-track-activity)
   - 6.2. [Steps in Enola-AI](#62-steps-in-enola-ai)
   - 6.3. [Feedback Evaluation](#63-feedback-evaluation)
   - 6.4. [Extracting Information](#64-extracting-information)
7. [Documentation: Sending Data to Enola-AI](#7-documentation-sending-data-to-enola-ai)
   - 7.1. [Sending Online Chat Data](#7-documentation-sending-data-to-enola-ai)
   - 7.2. [Sending Online Score Data](#7-documentation-sending-data-to-enola-ai)
   - 7.3. [Sending Multiple Tasks](#7-documentation-sending-data-to-enola-ai)
   - 7.4. [Sending File Information](#7-documentation-sending-data-to-enola-ai)
   - 7.5. [Sending API Information](#7-documentation-sending-data-to-enola-ai)
   - 7.6. [Sending Cost Information](#7-documentation-sending-data-to-enola-ai)
   - 7.7. [Sending Batch Score Information](#7-documentation-sending-data-to-enola-ai)
8. [Summary](#8-summary)
   - 8.1. [Building an Ollama Chatbot](#81-building-an-ollama-chatbot)
   - 8.2. [Frequently Asked Questions](#82-frequently-asked-questions)
   - 8.3. [Complete Code Examples](#83-complete-code-examples)
9. [Contributing](#9-contributing)
10. [License](#10-license)
11. [Contact](#11-contact)

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
    token=token,              # Your Enola API token
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

## 5.2. Complete Example: Basic Tracking Initialization

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

## 6. Understanding how Enola-AI works

To better understand how Enola-AI works and what has to offer, it is important that you understand its different features.
This section is going to be divided in 4 subcategories:

- [6.1. Track Activity](docs/track_activity.md)
- [6.2. Steps in Enola-AI](docs/steps_in_enola_ai.md)
- [6.3. Feedback Evaluation](docs/feedback_evaluation.md)
- [6.4. Extracting Information](docs/extracting_information.md)

---

### 6.1. Track Activity

When using Enola-AI, you can add Track Activity, allowing you to track any interactions from the system and users.
By tracking the interactions in your system with Enola-AI, you can effectively monitor, validate and evaluate your models.

[6.1. Track Activity](docs/track_activity.md)

---

### 6.2. Steps in Enola-AI 
In Enola-AI, the concept of **steps** is fundamental for tracking the execution flow of your AI agents. Each step represents a significant action or event in your agent's processing pipeline.

There are two main types of steps:

- **Generic Steps**: Used for general-purpose tracking of actions that are not specific to language models, such as data retrieval, preprocessing, or any custom logic.

- **LLM Steps**: Specifically designed for tracking interactions with Language Models (e.g., GPT-4, Ollama, BERT), where token usage and costs are relevant.

Understanding the difference between these step types is crucial for accurate tracking and cost analysis.

[6.2. Steps in Enola-AI](docs/steps_in_enola_ai.md)

---

### 6.3. Feedback Evaluation
Enola-AI provides a feedback system that allows users to evaluate AI agent executions. Feedback can be submitted either through the Enola-AI platform or programmatically via code. This helps in assessing the performance of your AI agents and gathering user insights for improvement. 

[6.3. Feedback Evaluation](docs/feedback_evaluation.md)

---

### 6.4. Extracting Information

Enola-AI allows you to retrieve the Input and Output data from past executions for analysis or further processing.

[6.4. Extracting Information](docs/extracting_information.md)

---

## 7. Documentation: Sending data to Enola-AI

In this section, you can find documentation about Enola-AI, including step-by-step instructions and examples, along with explanations of the system's functionalities:

For the complete documentation, you can visit our guide Sending Data to Enola-AI covering the following sections:
- [7. Sending Data to Enola-AI](docs/sending_data.md#7-documentation-sending-data-to-enola-ai)
   - [7.1. Sending Online Chat Data](docs/sending_data.md#71-sending-online-chat-data)
   - [7.2. Sending Online Score Data](docs/sending_data.md#72-sending-online-score-data)
   - [7.3. Sending Multiple Tasks](docs/sending_data.md#73-sending-multiple-tasks)
   - [7.4. Sending File Information](docs/sending_data.md#74-sending-file-information)
   - [7.5. Sending API Information](docs/sending_data.md#75-sending-api-information)
   - [7.6. Sending Cost Information](docs/sending_data.md#76-sending-cost-information)
   - [7.7. Sending Batch Score Data](docs/sending_data.md#77-sending-batch-score-information)

---

## 8. Summary

- This documentation provides a basic guide on using the Enola-AI Python library to initialize tracking and send data.
- For detailed documentation about sending data with Enola-AI, you can visit our [Sending Data to Enola-AI](docs/sending_data.md) guide.

### 8.1. Building an Ollama Chatbot
   - For a comprehensive guide on how to build a Chatbot using Ollama with Enola-AI Tracking, you can visit our section [Building an Ollama Chatbot](docs/building_chatbot_ollama.md).

### 8.2. Frequently Asked Questions
   - If you have questions, you are invited to visit our [Frequently Asked Questions](docs/faq.md) section.
   
### 8.3. Complete Code Examples
   - If you want to check all the complete code examples, you can visit our [Complete Code Examples](docs/complete_code_examples.md) section.

---

## 9. Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or corrections.

When contributing, please ensure to:

- Follow the existing coding style.
- Write clear commit messages.
- Update documentation as necessary.
- Ensure that any code changes are covered by tests.

---

## 10. License

This project is licensed under the **BSD 3-Clause License**.

---

## 11. Contact

For any inquiries or support, please contact us at [help@huemulsolutions.com](mailto:help@huemulsolutions.com).
