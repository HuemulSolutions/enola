# Complete Code Examples

In this section you will find the complete code examples used in the Enola-AI documentation.

**Note: Before using the provided codes, make sure you have:**

#### **Required Python Packages:**
Make sure you installed the Enola-AI Python library and necessary libraries, you can install it by running the following command in your command prompt or terminal.
```bash
pip install langchain enola dotenv uuid langchain_ollama
```
This will install Enola-AI library, other necessary libraries and LangChain (a framework that helps facilitate the integration of large language models into applications).

#### **Your Enola-AI token:**
To configure your token, you can follow the explanation in the [Getting Started](https://github.com/HuemulSolutions/Enola-AI#5-getting-started) section from the Enola-AI documentation.

#### **A functional Chatbot or AI model:**
You can use your own AI model or you can check our user guide to build a chatbot using Ollama, by visiting our section [Building an Ollama Chatbot](building_chatbot_ollama.md).
In the next code examples, when a chatbot is required, it is going to be imported in the following way:

```python
# example_code.py

# Import your model/chatbot function 
from ollama_chatbot import ollama_chat # ensure ollama_chatbot.py (or your own model) is available
```
This approach asssumes you have your chatbot/model function defined in a different script (e.g. ollama_chatbot.py).

Simple example of a chatbot/model class:
```python
# ollama_chatbot.py

# Import Langchain to handle Ollama through code
from langchain_ollama import OllamaLLM

# Define the ollama_chat function
def ollama_chat(prompt, model="llama3.2"): # define user question (prompt) and Ollama model version
    llm = OllamaLLM(model=model)
    response = llm.invoke(prompt)
    return response                        # return a model response after sending a user question
```

Then in your code, you can get a `response` when invoking `ollama_chat()` with the message from user `user_input`.
```python
# example_code.py

model_response = ollama_chat(user_input)  # the response from the external model/chatbot
```

**By understanding how to load your **Enola-AI token** and how to invoke a chatbot/model to obtain a `response`, you will be able to use any code example from Enola-AI documentation. If you have your own custom model, make sure to replace the respective `imports` and `functions` with your own.**

---

## Table of Contents

   - 6.3. [Feedback Evaluation](#63-feedback-evaluation)
   - 6.4. [Extracting Information](#64-extracting-information)
   - 7.1. [Sending Online Chat Data](#71-complete-example-sending-online-chat-data)
   - 7.2. [Sending Online Score Data](#72-complete-example-sending-online-score-data)
   - 7.3. [Sending Multiple Tasks](#73-complete-example-sending-multiple-tasks)
   - 7.4. [Sending File Information](#74-complete-example-sending-file-information)
   - 7.5. [Sending API Information](#75-complete-example-sending-api-information)
   - 7.6. [Sending Cost Information](#76-complete-example-sending-cost-information)
   - 7.7. [Sending Batch Score Information](#77-complete-example-sending-batch-score-information)
   - 8.1. [Building an Ollama Chatbot](#81-complete-example-building-an-ollama-chatbot)

---

#### **Complete Example: Extracting Information**

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
    limit=20
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

---
		
#### **Complete Example: Extract Information and Create Systematic Evaluations**


```python
# Import necessary libraries
from enola import get_executions
from enola import evaluation
from enola.enola_types import EvalType
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
    limit=20
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

        # Create an Evaluation Instance
        eval = evaluation.Evaluation(
            token=token,
            eval_type=EvalType.AUTO,
            user_id="auto_123",
            user_name="auto",
        )

        # Add an Evaluation
        eval.add_evaluation(
            enola_id=row.enola_id,
            eval_id="003",
            value=len(row.message_output),
            comment=""
        )

        result = eval.execute()
```

---

#### **7.1. Complete Example: Sending Online Chat Data**

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

---

#### **7.2. Complete Example: Sending Online Score Data**

```python
from enola.tracking import Tracking
from enola.enola_types import ErrOrWarnKind
from dotenv import load_dotenv
import os
import uuid

# Load .env file and set up your token
load_dotenv()
token = os.getenv('ENOLA_TOKEN')

# Generate a session_id
session_id = str(uuid.uuid4())

# Initialize the tracking object
monitor = Tracking(
    token=token,
    name="Online Scoring Session",
    is_test=True,
    app_id="OnlineScoringApp",
    user_id="User123",
    session_id=session_id,
    channel_id="API",
    ip="192.168.1.1",
    message_input="Score request"
)

# Create a new step for scoring
step_score = monitor.new_step("Online Scoring")

# Simulated input features
input_features = {
    "age": 35,
    "income": 55000,
    "credit_history": "Good",
    "loan_amount": 15000
}

# Add input features to the step
step_score.add_extra_info("InputFeatures", input_features)

# Simulated model score
model_score = 0.85  # Probability of default

# Add model score to the step
step_score.add_extra_info("ModelScore", model_score)

# Step: Validate Model Score
step_validate_score = monitor.new_step("Validate Model Score")
if not (0 <= model_score <= 1):
    step_validate_score.add_error(
        id="InvalidScore",
        message="Model score is out of valid range (0 to 1).",
        kind=ErrOrWarnKind.INTERNAL
    )
    monitor.close_step_others(
        step=step_validate_score,
        successfull=False,
        message_output="Invalid model score."
    )
    # Execute tracking with successfull=False
    monitor.execute(
        successfull=False,
        message_output="Invalid model score.",
        num_iteratons=2
    )
    print("Model score is invalid.")
    exit()  # Exit or handle accordingly
else:
    monitor.close_step_others(
        step=step_validate_score,
        successfull=True,
        message_output="Model score is valid."
    )

# Close the scoring step
monitor.close_step_others(
    step=step_score,
    successfull=True,
    message_output="Online scoring completed.",
    others_cost=0.0005  # Example cost per scoring
)

# Execute the tracking
monitor.execute(
    successfull=True,
    message_output="Online scoring session completed.",
    num_iteratons=2
)

# Continue with your application logic
print(f"Model Score: {model_score}")
```

---

#### **7.3. Complete Example: Sending Multiple Tasks**

```python
from enola.tracking import Tracking
from enola.enola_types import ErrOrWarnKind
from ollama_chatbot import ollama_chat  # Import your model/chatbot function
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

---

#### **7.4. Complete Example: Sending File Information**

```python
from enola.tracking import Tracking
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

---

#### **7.5. Complete Example: Sending API Information**

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

---

#### **7.6. Complete Example: Sending Cost Information**

```python
from enola.tracking import Tracking
from ollama_chatbot import ollama_chat  # Import your model/chatbot
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
    message_input=user_input
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
    message_output=response,
    num_iteratons=3  # Total number of steps
)

# Print question and answer from LLM invoke
print(user_input)
print(response)
```

---

#### **7.7. Complete Example: Sending Batch Score Information**

```python
from enola.tracking import Tracking
from enola.enola_types import ErrOrWarnKind
from dotenv import load_dotenv
import os
import uuid

# Load .env file and set up your token
load_dotenv()
token = os.getenv('ENOLA_TOKEN')

# Generate a session_id
session_id = str(uuid.uuid4())

# Initialize the tracking object
monitor = Tracking(
    token=token,
    name="Batch Scoring Session",
    is_test=True,
    app_id="BatchScoringApp",
    user_id="System",
    session_id=session_id,
    channel_id="BatchProcess",
    ip="127.0.0.1",
    message_input="Batch score processing started"
)

# Create a new step for batch processing
step_batch = monitor.new_step("Batch Score Processing")

# Simulated batch processing
total_records = 1000
processed_records = 995
failed_records = 5
processing_time = 120  # In seconds

# Add batch processing information to the step
batch_info = {
    "TotalRecords": total_records,
    "ProcessedRecords": processed_records,
    "FailedRecords": failed_records,
    "ProcessingTimeSeconds": processing_time
}
step_batch.add_extra_info("BatchProcessingInfo", batch_info)

# Simulated failed record IDs
failed_record_ids = [101, 202, 303, 404, 505]

if failed_records > 0:
    # Add an error indicating batch processing failures
    step_batch.add_error(
        id="BatchProcessingErrors",
        message=f"{failed_records} records failed to process.",
        kind=ErrOrWarnKind.EXTERNAL
    )
    # Add the failed record IDs as extra info
    step_batch.add_extra_info("FailedRecordIDs", failed_record_ids)

# Close the batch processing step
monitor.close_step_others(
    step=step_batch,
    successfull=(failed_records == 0),
    message_output="Batch processing completed.",
    others_cost=0.05  # Example cost
)

# Execute the tracking
monitor.execute(
    successfull=(failed_records == 0),
    message_output=f"Completed, {total_records} records, {failed_records} errors.",
    num_iteratons=1
)

# Continue with your application logic
if failed_records > 0:
    print(f"Batch Completed, {total_records} records, {failed_records} errors.")
else:
    print("Batch processing completed successfully.")

```

---

#### **8.1. Complete Example: Building an Ollama Chatbot**

```python
# Import necessary libraries
import os
import uuid
from dotenv import load_dotenv
from langchain_ollama import OllamaLLM

# Import Enola modules
from enola.tracking import Tracking
from enola.enola_types import ErrOrWarnKind

# Load .env file with the token inside
load_dotenv()

# Set up your Enola token
token = os.getenv("ENOLA_TOKEN")

# Generate a session_id (remains the same for the entire chat session)
session_id = str(uuid.uuid4())

# Define the function for invoking the answer from the model
def ollama_chat(prompt, model="llama3.2"):
    llm = OllamaLLM(model=model)
    response = llm.invoke(prompt)
    return response

# Define the chatbot functions
def chatbot():
    print("Welcome to the Ollama Chatbot!")
    print("Type 'exit' to end the conversation.")

    chat_active = True
    interaction_count = 0  # Counts the number of interactions

    while chat_active:
        user_input = input("You: ")
        if user_input.lower() == "exit":
            print("Goodbye!")
            chat_active = False
            continue  # Exit the loop

        interaction_count += 1

        # Initialize num_iteratons for this interaction
        num_iteratons = 0

        # Create a Tracking object for this interaction
        monitor = Tracking(
            token=token,
            name=f"Ollama Chatbot Interaction {interaction_count}",
            is_test=True,
            app_id="OllamaChatAppConsole",
            user_id="user@example.com",
            session_id=session_id,
            channel_id="console",
            ip="127.0.0.1",
            message_input=user_input  # User's input
        )

        # Step 1: Validate User Input
        num_iteratons += 1  # num_iteratons = 1
        step_validate_input = monitor.new_step("Validate User Input")

        if not user_input.strip():
            step_validate_input.add_error(
                id="EmptyUserInput",
                message="User input is empty.",
                kind=ErrOrWarnKind.EXTERNAL
            )
            monitor.close_step_others(
                step=step_validate_input,
                successfull=False,
                message_output="User input is empty."
            )
            print("Please enter a valid input.")

            # Execute the monitor with the current num_iteratons
            monitor.execute(
                successfull=False,
                message_output="User input is empty.",
                num_iteratons=num_iteratons
            )
            continue  # Skip to the next iteration
        else:
            monitor.close_step_others(
                step=step_validate_input,
                successfull=True,
                message_output="User input is valid."
            )

        # Step 2: User Interaction
        num_iteratons += 1  # num_iteratons = 2
        step_chat = monitor.new_step(f"User Interaction {interaction_count}")

        # Set the message_input attribute of the step to the user's input
        step_chat.message_input = user_input

        try:
            # Initialize variables
            file_content = None
            response = ""

            # Check if the user wants to upload a file
            if user_input.lower().startswith("upload file"):
                # Extract file path from the command if provided
                parts = user_input.split(maxsplit=2)
                if len(parts) == 3:
                    file_path = parts[2]
                else:
                    file_path = input("Please enter the file path: ")

                # Process the file
                try:
                    with open(file_path, 'r') as file:
                        file_content = file.read()

                    # Add file information to the step
                    step_chat.add_file_link(
                        name=os.path.basename(file_path),
                        url=file_path,
                        type=os.path.splitext(file_path)[1],
                        size_kb=os.path.getsize(file_path) / 1024,
                        description=f"User uploaded file from local path: {file_path}"
                    )

                    # Simulate processing the file content
                    response = f"File '{os.path.basename(file_path)}' uploaded successfully."

                except Exception as e:
                    print(f"Error reading file: {e}")
                    file_content = None
                    response = f"An error occurred while reading the file: {e}"

            else:
                # API call details
                api_name = "Ollama LLM API"
                method = "POST"
                url = "http://localhost:11434/"  # API Endpoint
                headers_sent = {"Content-Type": "application/json"}
                body_sent = {"model": "llama3.2", "prompt": user_input}

                # Add API data to the step before making the call
                import json
                step_chat.add_api_data(
                    name=api_name,
                    method=method,
                    url=url,
                    bodyToSend=json.dumps(body_sent),
                    headerToSend=json.dumps(headers_sent),
                    payloadReceived="",  # Will be updated after the response
                    description="Call to Ollama LLM API."
                )

                # Invoke the LLM to get the response
                response = ollama_chat(user_input)

                # Update payloadReceived with the actual response
                step_chat.add_api_data(
                    name=api_name,
                    method=method,
                    url=url,
                    bodyToSend=json.dumps(body_sent),
                    headerToSend=json.dumps(headers_sent),
                    payloadReceived=response,  # Actual response
                    description="Call to Ollama LLM API."
                )

            # Simulate token counts (replace with actual counts if possible)
            token_input_num = len(user_input.split())  # Simple estimation
            token_output_num = len(response.split())   # Simple estimation
            token_total_num = token_input_num + token_output_num

            # Assume costs (replace with actual calculations if available)
            token_input_cost = token_input_num * 0.0002
            token_output_cost = token_output_num * 0.0002
            token_total_cost = token_input_cost + token_output_cost

            # Close the interaction step with token usage and costs
            monitor.close_step_token(
                step=step_chat,
                successfull=True,
                message_output=response,
                token_input_num=token_input_num,
                token_output_num=token_output_num,
                token_total_num=token_total_num,
                token_input_cost=token_input_cost,
                token_output_cost=token_output_cost,
                token_total_cost=token_total_cost
            )

            # Step 3: Validate Model Response
            num_iteratons += 1  # num_iteratons = 3
            step_validate_response = monitor.new_step("Validate Model Response")
            if not response.strip():
                step_validate_response.add_error(
                    id="EmptyModelResponse",
                    message="Model response is empty.",
                    kind=ErrOrWarnKind.INTERNAL
                )
                monitor.close_step_others(
                    step=step_validate_response,
                    successfull=False,
                    message_output="Model response is empty."
                )
                print("An error occurred while generating the response.")

                # Execute the monitor with the current num_iteratons
                monitor.execute(
                    successfull=False,
                    message_output="Model response is empty.",
                    num_iteratons=num_iteratons
                )
                continue  # Skip to the next iteration
            else:
                monitor.close_step_others(
                    step=step_validate_response,
                    successfull=True,
                    message_output="Model response is valid."
                )

            # Add user's question and model's response to the step
            step_chat.add_extra_info("UserInput", user_input)
            step_chat.add_extra_info("ModelResponse", response)

            print(f"Chatbot: {response}")

        except Exception as e:
            # Register the error in Enola
            step_chat.add_error(
                id="LLMInvocationError",
                message=str(e),
                kind=ErrOrWarnKind.EXTERNAL
            )
            # Close the step as unsuccessful
            monitor.close_step_token(
                step=step_chat,
                successfull=False,
                message_output="Error occurred during LLM invocation."
            )

            # Execute the monitor with the current num_iteratons
            monitor.execute(
                successfull=False,
                message_output="Error occurred during LLM invocation.",
                num_iteratons=num_iteratons
            )

            print("An error occurred while processing your request.")
            continue  # Skip to the next iteration

        # After processing the interaction, execute the monitor to send the data
        monitor.execute(
            successfull=True,  # Overall success status of the interaction
            message_output=response,
            num_iteratons=num_iteratons  # Total number of steps in this interaction
        )

if __name__ == "__main__":
    chatbot()
```