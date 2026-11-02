## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

### DESIGN STEPS:

#### STEP 1: Import the required Python libraries and configure the OpenAI API key using an environment variable.

#### STEP 2: Define the calculate_cylinder_volume() function and create the function schema with radius and height as parameters.

#### STEP 3: Integrate the function with the OpenAI Chat Completion API, execute the function call, return the calculated result to the LLM and display the final output.

### PROGRAM:
```
import os
import json
import math
import openai

openai.api_key = os.getenv("OPENAI_API_KEY")

def calculate_cylinder_volume(radius, height):
    volume = math.pi * radius ** 2 * height
    return round(volume, 2)

functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate the volume of a cylinder",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {"type": "number"},
                "height": {"type": "number"}
            },
            "required": ["radius", "height"]
        }
    }
]

messages = [
    {
        "role": "user",
        "content": "Calculate the volume of a cylinder with radius 3 and height 5"
    }
]

response = openai.ChatCompletion.create(
    model="gpt-4o-mini",
    messages=messages,
    functions=functions,
    function_call="auto"
)

print(response)

message = response["choices"][0]["message"]

function_name = message["function_call"]["name"]

arguments = json.loads(message["function_call"]["arguments"])

result = calculate_cylinder_volume(
    arguments["radius"],
    arguments["height"]
)

print("Function:", function_name)
print("Radius:", arguments["radius"])
print("Height:", arguments["height"])
print("Volume:", result)

messages.append(message)

messages.append({
    "role": "function",
    "name": function_name,
    "content": json.dumps({"volume": result})
})

final_response = openai.ChatCompletion.create(
    model="gpt-4o-mini",
    messages=messages
)

print(final_response["choices"][0]["message"]["content"])


```

### OUTPUT:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9aa8fd73-c8a7-43f5-b3b2-cb4573f86a64" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/673cda4d-f110-453e-9b80-5d9a62254b65" />


### RESULT:
Thus, the Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling is Successful.
