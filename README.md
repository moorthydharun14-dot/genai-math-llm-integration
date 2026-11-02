# Integration of Mathematical Calculations with a Chat Completion System using LLM Function-Calling

## AIM:
To design and implement a Python function to calculate the area of a regular hexagon and integrate it with a chat completion system using the function-calling feature of a Large Language Model (LLM).

## PROBLEM STATEMENT:
To calculate the area of a regular hexagon for a given side length using Python and enable an LLM to call the mathematical function automatically.

## DESIGN STEPS:

**STEP 1:** Import the required libraries and load the OpenAI API key using environment variables.

**STEP 2:** Define a Python function to calculate the area of a regular hexagon using the formula \(A = \frac{3\sqrt{3}}{2} \times s^2\), and configure the function as an LLM tool.

**STEP 3:** Send the user's question to the LLM, process the function call, calculate the area, and display the result.

## PROGRAM:
```
import os
import json
import math
from dotenv import load_dotenv, find_dotenv
from openai import OpenAI

# Load API key
load_dotenv(find_dotenv())
client = OpenAI(api_key=os.environ["OPENAI_API_KEY"])

# Define the mathematical function
def calculate_hexagon_area(side):
    """Calculate the area of a regular hexagon."""
    area = (3 * math.sqrt(3) / 2) * side**2

    return json.dumps({
        "side": side,
        "area": round(area, 2)
    })

# Define the tool
tools = [
    {
        "type": "function",
        "name": "calculate_hexagon_area",
        "description": "Calculate the area of a regular hexagon",
        "parameters": {
            "type": "object",
            "properties": {
                "side": {"type": "number"}
            },
            "required": ["side"],
            "additionalProperties": False
        },
        "strict": True
    }
]

# Ask the question
response = client.responses.create(
    model="gpt-5.5",
    input="Calculate the area of a regular hexagon with side length 5.",
    tools=tools,
    tool_choice="required"
)

# Process the tool call
for item in response.output:
    if item.type == "function_call":
        args = json.loads(item.arguments)

        result = calculate_hexagon_area(**args)

        print("Function arguments:", args)
        print("Calculated area:", result)
```

## OUTPUT:
<img width="1496" height="712" alt="image" src="https://github.com/user-attachments/assets/bfe77b5b-ec5e-4b08-837c-8d19cadc7e19" />


## RESULT:
Thus, the Python program to calculate the area of a regular hexagon was implemented successfully using LLM function-calling.
