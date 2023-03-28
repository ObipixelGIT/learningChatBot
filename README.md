# learningChatBot
A learning ChatBot written in Python 3.

## How this script works?
When using this ChatBot and you ask questions, it will respond to you not knowing how to answer.
It will ask how to answer the question and it will learn from your response.
When you ask the question again, it will answer correctly based on your response.

## Preparation
Install the libaries:
```bash
pip3 install random, string, typing
```

## Permissions

Ensure you give the script permissions to execute. Do the following from the terminal:
```bash
sudo chmod +x learningChatBot.py
```

## Usage
```bash
sudo python3 learningChatBot.py
```

## How to teach the learningChatBot?
```
Welcome to the Python chat bot! Type 'exit' to quit.
> What is your name?
I'm sorry, I don't understand.
Was that helpful? [y/n] n
What should I have said? My name is James!
> What is your name?
My name is James!
Was that helpful? [y/n] y
> 
```

## Example script
```python
import random
import string
from typing import Dict, List

# Define a dictionary to store the user's input and the bot's response
chat_history: Dict[str, List[str]] = {}

# Define a list of responses to use when the bot doesn't have a learned response
default_responses: List[str] = [
    "I'm sorry, I don't understand.",
    "Could you please rephrase that?",
    "I'm not sure what you're asking.",
]


# Define a function to clean the user's input by removing punctuation and converting to lowercase
def clean_input(user_input: str) -> str:
    cleaned_input = user_input.translate(str.maketrans("", "", string.punctuation)).lower()
    return cleaned_input


# Define a function to generate a response score based on the similarity of two strings
def score_response(user_input: str, bot_response: str) -> int:
    cleaned_user_input = clean_input(user_input)
    cleaned_bot_response = clean_input(bot_response)
    score = 0
    for word in cleaned_user_input.split():
        if word in cleaned_bot_response:
            score += 1
    return score


# Define a function to generate a response
def generate_response(user_input: str) -> str:
    # Clean the user input
    cleaned_input = clean_input(user_input)

    # Check if the user input is already in the chat history
    if cleaned_input in chat_history:
        # If it is, return the highest-scoring bot response
        bot_responses = chat_history[cleaned_input]
        scored_responses = [(response, score_response(user_input, response)) for response in bot_responses]
        scored_responses.sort(key=lambda x: x[1], reverse=True)
        return scored_responses[0][0]
    else:
        # If it's not, randomly select a default response from the list
        response = random.choice(default_responses)
        # Store the user input and the bot response in the chat history
        chat_history[cleaned_input] = [response]
        return response


# Define a function to update the chatbot's responses with new input
def update_responses(user_input: str, bot_response: str) -> None:
    cleaned_input = clean_input(user_input)
    if cleaned_input in chat_history:
        chat_history[cleaned_input].append(bot_response)
    else:
        chat_history[cleaned_input] = [bot_response]


# Define a main function to handle user input and generate responses
def main() -> None:
    print("Welcome to the Python chat bot! Type 'exit' to quit.")
    # Loop until the user types 'exit'
    while True:
        # Prompt the user for input
        user_input = input("> ")
        # If the user types 'exit', break out of the loop
        if user_input == "exit":
            break
        # Otherwise, generate a response and print it
        response = generate_response(user_input)
        print(response)
        # Ask the user if the response was helpful and learn from the feedback
        feedback = input("Was that helpful? [y/n] ")
        if feedback.lower() == "n":
            correct_response = input("What should I have said? ")
            update_responses(user_input, correct_response)


# Call the main function to start the chat bot
main()
```

## License Information

This library is released under the [Creative Commons ShareAlike 4.0 International license](https://creativecommons.org/licenses/by-sa/4.0/). You are welcome to use this library for commercial purposes. For attribution, we ask that when you begin to use our code, you email us with a link to the product being created and/or sold. We want bragging rights that we helped (in a very small part) to create your 9th world wonder. We would like the opportunity to feature your work on our homepage.
