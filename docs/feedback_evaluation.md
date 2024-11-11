## 6.3. Feedback Evaluation 

Enola-AI provides a feedback system that allows users to evaluate AI agent executions. Feedback can be submitted either through the Enola-AI platform or programmatically via code. This helps in assessing the performance of your AI agents and gathering user insights for improvement.

![Feedback Evaluation](images/feedback_evaluation.jpg)

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
Evaluation types:
- USER: Corresponds to evaluations made by the user after an interaction.
- AUTO: Corresponds to evaluations that are made automatically by the system.
- INTERNAL: Corresponds to evaluations made from internal users when monitoring evaluations.
 
```python
eval = evaluation.Evaluation(
    token=token,
    eval_type=EvalType.INTERNAL,
    user_id="internal_123",
    user_name="Internal User",
)
```

### Step 4: Add an Evaluation

Provide the execution ID (`enola_id`), an ID from the evaluation category (`eval_id`) , an evaluation value (`value`) and a `comment`.
In the Enola-AI platform, you can create different categories for evaluations, each category can have its own ranges of values. For example, you can have an evaluation category "Format of the Response", with its own ID `eval_id`, a value inside a range to determine the quality of an interaction, and a `comment`.

Practical example:
You have to evaluate the "Format of the Response".

- Evaluation Category: "Format of the Response"
- Evaluation Category ID (`eval_id`): "02"
- Evaluation Value (`value`): 95
- Comment (`comment`): "The response was informative and well-structured."

Code example:
```python
eval.add_evaluation(
    enola_id="g57y84hkc412m", # Provide the execution ID (`enola_id`)
    eval_id="02", 			  # Provide the ID from a category
    value=95,  				  # Add a score value between a custom range
    comment="The response was informative and well-structured."
)
```

In this example, the ranges are configured in the following way (High score is Very Good and Low Score is Very Bad):
- 0-20: Very Bad
- 21-40: Bad
- 41-60: Intermediate
- 61-80: Good
- 81-100: Very Good

This means a `value=95` will be considered as Very Good for the "Format of the Response".

**Note: Inside the Enola-AI Platform, you can create and choose the Category name, the ID for `eval_id`, the different ranges for `value`, and a description**

### Step 5: Add an Evaluation by Level

You can also provide an evaluation by level instead of a value. The evaluation by `level` has a value between 1 (Very Bad) and 5 (Very Good).

```python
eval.add_evaluation_by_level(
    enola_id="g57y84hkc412m", # Provide the execution ID (`enola_id`)
    eval_id="02",			  # Provide the ID from a category
    level=5,				  # Add a score level between 1 and 5
    comment="Good response, helpful and concise."
)
```

### Step 6: Execute the Evaluation Submission

Submit the evaluation to the Enola-AI platform.

```python
result = eval.execute()
```

#### **Complete Example: Feedback Evaluation**
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
    eval_type=EvalType.INTERNAL,
    user_id="internal_123",
    user_name="Internal User",
)

# Add an Evaluation
eval.add_evaluation(
    enola_id="g57y84hkc412m", # Provide the execution ID (`enola_id`)
    eval_id="02", 			  # Provide the ID from a category
    value=95,  				  # Add a score value between a custom range
    comment="The response was informative and well-structured."
)

# Add an Evaluation by Level
eval.add_evaluation_by_level(
    enola_id="k90h55mqc342s", # Provide the execution ID (`enola_id`)
    eval_id="02",			  # Provide the ID from a category
    level=5,				  # Add a score level between 1 and 5
    comment="Good response, helpful and concise."
)

# Execute the Evaluation Submission
result = eval.execute()
```

By following these steps, you can send feedback for evaluations using Python.

### Managing Feedback on the Enola-AI Platform

You can log in to the [Enola-AI platform](https://enola-ai.com/) to view and manage your evaluations. There you can create feedback evaluations, by choosing an interaction in the Agent Executions section and clicking the icon "Create Evaluation". Then you can send the feedback and choose the corresponding parameters.

---