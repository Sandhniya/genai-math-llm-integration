## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
Design and implement a system where a user can input dimensions of a cylinder (radius and height),
and the system calculates its volume by invoking a Python function using the function-calling capabilities of an LLM.

### DESIGN STEPS:
```
1.Import necessary libraries, including OpenAI for LLM integration and math for mathematical operations.
2.Define a Python function to calculate the simple interest.
3.Integrate the function into an LLM-based chat completion system with function-calling capabilities.
```
### PROGRAM:
```
import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

import json

def get_simple_interest(principal, rate, time, unit="years"):
    """Return simple interest calculation in JSON format"""
    si = (principal * rate * time) / 100
    
    interest_info = {
        "concept": "Simple Interest",
        "principal": principal,
        "rate (%)": rate,
        "time": f"{time} {unit}",
        "formula": "(P × R × T) / 100",
        "simple_interest": si,
        "total_amount": principal + si
    }
    
    return json.dumps(interest_info)

```

```
# Call the ChatCompletion endpoint
response = openai.ChatCompletion.create(
    # OpenAI Updates: As of June 2024, we are now using the GPT-3.5-Turbo model
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions
)

# define a function
functions = [
    {
        "name": "get_simple_interest",
        "description": "Calculate the simple interest for given principal, rate, and time",
        "parameters": {
            "type": "object",
            "properties": {
                "principal": {
                    "type": "number",
                    "description": "The principal amount, e.g., 1000"
                },
                "rate": {
                    "type": "number",
                    "description": "The rate of interest (in percentage), e.g., 5"
                },
                "time": {
                    "type": "number",
                    "description": "The time duration for which interest is calculated"
                },
                "unit": {
                    "type": "string",
                    "enum": ["days", "months", "years"],
                    "description": "The unit of time"
                }
            },
            "required": ["principal", "rate", "time"]
        },
    }
]

```
```
print(response)
```
```
response_message = response["choices"][0]["message"]
response_message

```
```
response_message["content"]
response_message["function_call"]
json.loads(response_message["function_call"]["arguments"])
```
```
messages = [
    {
        "role": "user",
        "content": "Calculate the simple interest for principal 1000, rate 5%, and time 2 years"
    }
]

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call={"name": "get_simple_interest"},
)

print(response)

```
### OUTPUT:

<img width="754" height="908" alt="image" src="https://github.com/user-attachments/assets/b8193d34-73a9-4a37-bd1b-6d89d90660a1" />
<img width="611" height="213" alt="image" src="https://github.com/user-attachments/assets/6b4284ca-97d9-45a4-8cdb-a1f6c3138ad8" />
<img width="583" height="651" alt="image" src="https://github.com/user-attachments/assets/064dd859-0079-430e-9ce9-1d023da91963" />
<img width="732" height="722" alt="image" src="https://github.com/user-attachments/assets/550c7e75-ef36-42c3-bfea-c488c2570ff9" />
<img width="701" height="581" alt="image" src="https://github.com/user-attachments/assets/23cebaf0-f01f-42ee-b399-d334156262a6" />


### RESULT:
The integration of the simple interest calculation function with the LLM-based chat completion system was successfully implemented.
