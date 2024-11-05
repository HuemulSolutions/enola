## Frequently Asked Questions (FAQ)

## Table of Contents

1. [Frequently Asked Questions](#1-frequently-asked-questions)
   - 1.1. [How secure is my data with Enola-AI?](#11-how-secure-is-my-data-with-enola-ai)
   - 1.2. [How does the token work? How can I obtain the token?](#12-how-does-the-token-work-how-can-i-obtain-the-token)
   - 1.3. [What types of data can I track with Enola-AI?](#13-what-types-of-data-can-i-track-with-enola-ai)
   - 1.4. [How to handle the errors with Enola-AI?](#14-how-to-handle-the-errors-with-enola-ai)
   - 1.5. [How to use Steps? What is the proper way to open and close them?](#15-how-to-use-steps-what-is-the-proper-way-to-open-and-close-them)
   - 1.6. [When Sending File Information, can I check the content inside the file?](#16-when-sending-file-information-can-i-check-the-content-inside-the-file)
   - 1.7. [If I start the method tracking(), but the method execute() fails to execute, what happens with the data?](#17-if-i-start-the-method-tracking-but-the-method-execute-fails-to-execute-what-happens-with-the-data)
   - 1.8. [How does the graphic interface work in Enola-AI?](#18-how-does-the-graphic-interface-work-in-enola-ai)
   - 1.9. [How to link a model or chatbot with Enola-AI?](#19-how-to-link-a-model-or-chatbot-with-enola-ai)
   - 1.10. [Does Enola-AI support integrations with other platforms or tools?](#110-does-enola-ai-support-integrations-with-other-platforms-or-tools)

## 1. **Frequently Asked Questions**

### 1.1 **Question: How secure is my data with Enola-AI?**

**Answer:**

Enola-AI prioritizes data security and privacy:

- **Data Encryption:** All data transmitted to and from Enola-AI is encrypted using industry-standard protocols.
- **Access Control:** Your data is accessible only to you and authorized team members.
- **Privacy Compliance:** Enola-AI adheres to data protection regulations to ensure your data is handled responsibly.

---

### 1.2. **Question: How does the token work? How can I obtain the token?**

**Answer:**

The token in Enola-AI is a secure credential that authenticates your application with the Enola-AI platform. It ensures that only authorized applications can send data to your account. Here's how it works and how you can obtain it:

1. **Purpose of the Token:**

   - Acts as an authentication mechanism between your application and the Enola-AI servers.
   - Ensures secure communication and data integrity.
   - Is linked to your Enola-AI account and carries your permissions.

2. **Obtaining the Token:**

   - **Sign Up in https://enola-ai.com/**
   - After signing up, you will receive a token From Enola-AI.


3. **Security Considerations:**

   - **Keep It Secret:**

     Do not share your token publicly or commit it to version control systems.

By properly obtaining and securing your token, you ensure that your application's communications with Enola-AI are authenticated and secure.

---

### 1.3. **Question: What types of data can I track with Enola-AI?**

**Answer:**

Enola-AI allows you to track various types of data, including:

- **User Inputs and Outputs:** Messages sent by users and responses from your model.
- **Errors and Warnings:** Any exceptions or issues that occur during execution.
- **Performance Metrics:** Token usage, execution times, and costs.
- **File Information:** Metadata about files processed (name, size, type), without storing the actual content.
- **API Calls:** Details of external API requests and responses made by your application.
- **Custom Data:** Additional data and information relevant to your application.

---

### 1.4. **Question: How to handle the errors with Enola-AI?**

**Answer:**

In Enola-AI, you handle errors by logging them within the steps where they occur. This is done using the `add_error`. By logging errors, you can track issues and exceptions that happen during the execution of your application. Here's how you can handle errors:

1. **Add an Error to the Step:**

   ```python
   step.add_error(
       id="ErrorID",
       message="Description of the error.",
       kind=ErrOrWarnKind.EXTERNAL  # Use INTERNAL or EXTERNAL based on the error source
   )
   ```

   - **`id`**: A unique identifier for the error.
   - **`message`**: A descriptive message about the error.
   - **`kind`**: The kind of error, either `ErrOrWarnKind.INTERNAL` for internal errors or `ErrOrWarnKind.EXTERNAL` for external errors.

2. **Close the Step as Unsuccessful:**

   After logging the error, close the step and indicate that it was unsuccessful:

   ```python
   monitor.close_step_others(
       step=step,
       successfull=False,
       message_output="Error occurred during this step."
   )
   ```

3. **Handle Exceptions:**

   Use try-except blocks to catch exceptions and log them accordingly:

   ```python
   try:
       # Your code that might raise an exception
   except Exception as e:
       step.add_error(
           id="ExceptionID",
           message=str(e),
           kind=ErrOrWarnKind.INTERNAL
       )
       monitor.close_step_others(
           step=step,
           successfull=False,
           message_output="An exception occurred."
       )
   ```

By properly handling and logging errors, you maintain a detailed record of any issues that occur, which aids in debugging and improving your application's reliability.

---

### 1.5. **Question: How to use Steps? What is the proper way to open and close them?**

**Answer:**

In Enola-AI, steps are used to represent discrete actions or processes within your application's execution flow. Proper use of steps involves creating, populating, and closing them correctly. Here's how you can use steps:

1. **Create a New Step:**

   Use the `new_step` method to open a new step:

   ```python
   step = monitor.new_step("Step Name")
   ```

   - **`"Step Name"`**: A descriptive name for the step.

2. **Perform Actions and Log Data:**

   Within the step, perform your intended actions and log any relevant data:

   - **Add Extra Information:**

     ```python
     step.add_extra_info("Key", "Value")
     ```

   - **Log Errors or Warnings if Necessary:**

     ```python
     step.add_error(
         id="ErrorID",
         message="Error message.",
         kind=ErrOrWarnKind.INTERNAL
     )
     ```

3. **Close the Step:**

   After completing the actions, close the step using the appropriate method:

   - **For Generic Steps:**

     ```python
     monitor.close_step_others(
         step=step,
         successfull=True,  # Set to False if the step failed
         message_output="Outcome of the step."
     )
     ```

   - **For LLM Steps (involving language models):**

     ```python
     monitor.close_step_token(
         step=step,
         successfull=True,  # Set to False if the step failed
         message_output="Model response.",
         token_input_num=number_of_input_tokens,
         token_output_num=number_of_output_tokens,
         token_total_cost=total_token_cost,
         token_input_cost=input_token_cost,
         token_output_cost=output_token_cost
     )
     ```

   - **Parameters:**
     - **`successfull`**: Indicates whether the step was successful.
     - **`message_output`**: A message or result from the step.
     - **Token Parameters**: Relevant for LLM steps to track token usage and costs.

By properly opening and closing steps, you enable Enola-AI to accurately track the execution flow and analyze the performance and outcomes of each part of your application.

---

### 1.6. **Question: When Sending File Information, can I check the content inside the file?**

**Answer:**

The content inside the file cannot be checked in Enola-AI, this is due to security and privacy reasons. When uploading files, Enola-AI logs the information of the file, but not its content. This means the files you are uploading must be in its own repository.

---

### 1.7. **Question: If I start the method tracking(), but the method execute() fails to execute, what happens with the data?**

**Answer:**

If the Tracking started but the method execute() fails, it means the data won't be send to the Enola-AI servers. It is important that you execute this function if you want your data to be processed by Enola-AI. It is strongly recommended to handle exceptions in every section of your code, to avoid losing your data and keeping track of the potential errors that may appear during the execution of the different stages in a system.

---

### 1.8. **Question: How does the graphic interface work in Enola-AI?**

**Answer:**

You can visit https://enola-ai.com/ and Login with your Enola credentials. Inside the Enola-AI platform, you can check and review all your trackings sessions in section "Agent Executions". There it is possible to track any step or interaction that has been logged. Allowing you to monitor and evaluate your models.


---

### 1.9. **Question: How to link a model or chatbot with Enola-AI?**

**Answer:**

Linking your model or chatbot with Enola-AI involves integrating the Enola-AI tracking into your application's codebase. Here's how you can do it:

You have to import the class or the method of your model/chatbot into your code.
For this purpose, it is recommended to use Langchain (a framework that helps facilitate the integration of large language models into applications).

Example:

1. **Import the Langchain library for Ollama**
    ```python
    # Import Langchain for Ollama
    from langchain_ollama import OllamaLLM
    ```
2. **Call your model in your script**
    ```python
    # Import the method of your model
    def ollama_chat(prompt, model="llama3.2"):
        llm = OllamaLLM(model=model)
        response = llm.invoke(prompt)
        return response
    ```

3. **Call the model inside your code and store the response in a variable:**

    ```python
    # Invoke the LLM to get the response
    response = ollama_chat(user_input)
    ```

This will allow you to easily store the response from your model into the variable `response`.

For a comprehensive guide and example, please refer to our documentation on [Building a Chatbot using Ollama](docs/building_chatbot_ollama.md), which walks you through the process of integrating a chatbot with Enola-AI step by step.

---

### 1.10. **Question: Does Enola-AI support integrations with other platforms or tools?**

**Answer:**

Enola-AI is designed to be flexible and extensible:

- **API Access:** Use Enola-AI's API to integrate with other tools or platforms.
- **Custom Integrations:** You can build your own custom integrations based on your application's requirements.

---

If you need further information on any of these topics or have other questions, please let us know, and we'll be happy to assist you!
You can contact us at [help@huemulsolutions.com](mailto:help@huemulsolutions.com).