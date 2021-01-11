# ChatbotCreator

ChatbotCreator is a python package for creating chatbots(Including discord bots). 

## Installation

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install ChatbotCreator.

```bash
pip install chatbot-creator
```
## Data format

First you need to create a json data file and save it as "data.json" in the current directory for creating the machine learning model.

example:
```json
{"data": [
        {"class": "greeting",
         "patterns": ["Hi", "How are you", "Is anyone there?", "Hello", "Good day"],
         "responses": ["Hello!", "Good to see you again!", "Hi there, how can I help?"]
        },
        {"class": "goodbye",
         "patterns": ["cya", "See you later", "Goodbye", "I am Leaving", "bye"],
         "responses": ["Sad to see you go :(", "Talk to you later", "Goodbye!"]
        },
        {"class": "name",
         "patterns": ["what is your name", "what should I call you", "whats your name"],
         "responses": ["You can call me Nacky.", "I'm Nacky!"]
        }
   ]
}
```
here "class" is the label, "patterns" are the questions for the label and "responses" are the responses or answer for the label.

[NOTE]: You should create the "data.json" file like the given example where it should first have the "data" tag and then other tags exlained in the example.

## Usage 

To create a simple chatbot, first you need to create the pickle data files

### Creating a neural network model.

```python
import ChatbotCreator
from ChatbotCreator import ChatbotCreator

main = ChatbotCreator("model.hdf5") # Model file name to save
main.createData("data.json") # The data file that you created. This will create the pickle data files.
main.createModel(use_neural_network=True)'''This will create a neural network model and save it 
with the name that you gave in the first step.
If you want to create a SVM model then you can set that value to False. 
'''
```
NOTE: Currently, you can't create a discord bot with a SVM model but you can create a normal bot with SVM.
After you have created the pickle data files and the model using the above code, you can remove that.

```python
from ChatbotCreator import Run

run_model = Run("model.hdf5", used_neural_network=True)''' Enter the model file name and specify whether you have used a neural network model or not.'''

while True:
    inp = input("Enter cmd: ")
    if inp == "q":
        break
    pred, response, results, results_index = run_model.run(input_variable=inp) ''' specify the input variable through which you will parse in the input values.
    '''
    print(response)
    # this will run the model
```

 Here- "pred" is the predicted label, "response" is the response for the predicted label, results are the probabilities for every single label and "results_index" is the predicted label's index in "results"

 ### Creating a SVM model

 ```python
import ChatbotCreator
from ChatbotCreator import ChatbotCreator

main = ChatbotCreator("model.hdf5")
main.createData("data.json")
main.createModel(use_neural_network=False)
```

```python
while True:
    inp = input("Enter cmd: ")
    if inp == "q":
        break
    run_model = Run("model.hdf5", used_neural_network=False)
    pred, response = run_model.run(input_variable=inp) 
    print(response)
```

## Creating a discord bot

To create a discord bot:

```python
from ChatbotCreator import CreateDiscordBot

discord_bot = CreateDiscordBot("model_file_name", "bot_token", use_wikipedia=True)

'''
model_file_name is the name of the model file, bot_token is the bot_token that you can get in discord and when use_wikipedia is set to True, it will send wikipedia results when the model is not confident about a particular question.
'''

discord_bot.run() # this will run the discord bot

```

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## Issues and Problems

Currently, the CreateDiscordBot class has some issues which are:

    1st issue) It can't be ran as a python notebook, it raises an error which needs help. It can only be ran as a python script.(class CreateDiscordBot)

    2nd problem) When running the discord bot, it will function normally and send messages to the discord server for the queries but it will not return the predicted label to us.(class CreateDiscordBot) 

## License

MIT License

