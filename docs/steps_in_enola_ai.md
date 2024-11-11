## 6.2. Steps in Enola-AI

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

By understanding steps in Enola-AI, you will be able to create, manage and close steps, allowing you to track and set the interactions inside your AI system.