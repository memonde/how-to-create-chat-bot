This document outlined the process of creating a chatbot using:
1. OpenAI API gpt-3.5-turbo model
2. Telegram Bot
and also how to run the chatbot on a testing environment, for example Google Colab.
If you already know how to develop the code and how to use Google Colab, you can jump to the next step: [[Create a Github Repository]](https://github.com/memonde/how-to-create-chat-bot/blob/main/Create%20a%20Github%20Repository.md) .
If you already have a Github Repository for the chatbot and just look for instruction to set up a virtual machine, you can jump to [[Install and Run Chat Bot on Cloud Compute Engine]](https://github.com/memonde/how-to-create-chat-bot/blob/main/Install%20and%20Run%20Chat%20Bot%20on%20Cloud%20Compute%20Engine.md).

## Create a chatbot on Telegram
* Open account https://t.me/BotFather
* Enter command /newbot
* Set up your bot and get bot token
## Get OpenAI API Key
* Create or logon to OpenAI account: platform.openai.com/account
* Go to https://platform.openai.com/account/api-keys
* Create new secret key
	* Please copy and save your secret key somewhere immediately; you will not be able to see it once it is created. 

## Generate Python for chatbot

Below is a simple version of Telegram bot powered by GPT3.5-Turbo:
```python
import logging

import openai

from telegram import Update

from telegram.ext import Updater, CommandHandler, MessageHandler, Filters, CallbackContext

  

# Replace with your OpenAI API key

openai.api_key = "INSERT YOUR OPENAI API KEY"

# Replace with your Telegram bot token

telegram_bot_token = "INSERT BOT TOKEN PROVIDED BY BOTFATHER"

  

# Enable logging

logging.basicConfig(

format='%(asctime)s - %(name)s - %(levelname)s - %(message)s', level=logging.INFO

)

logger = logging.getLogger(__name__)

  

def start(update: Update, context: CallbackContext):

update.message.reply_text('Hello! How can I help you?')
# You can change the text to anything. It'll be the greeting your bot send out.

  

def chat_with_gpt3(text: str, conversation: list) -> str:

conversation.append({"role": "user", "content": text})

conversation = conversation[-8:]
  # Truncate conversation to fit within model's context length to avoid chat exceed the token length.

response = openai.ChatCompletion.create(

model="gpt-3.5-turbo",

messages=conversation,

temperature=1,

max_tokens=600, 

top_p=1,

frequency_penalty=0,

presence_penalty=0

)



conversation.append({"role": "assistant", "content": 				 
response.choices[0].message['content'].strip()})

return response.choices[0].message['content'].strip()

  

def chat(update: Update, context: CallbackContext):

user_id = update.effective_user.id

user_text = update.message.text

  

if user_id not in context.user_data:

context.user_data[user_id] = [{"role": "system", "content": "You are a data scientist, and proficient on SQL, Python, R2 and Visio."
# This is the prompt to anchor the basic persona of this bot. You can change to anything, but keep the language simple and straightforward. If you enter a long string of text, break them down into multiple lines; make sure each line starts with " and end with ", and only the last time end with "}] to keep  the syntax correct.

  

response = chat_with_gpt3(user_text, context.user_data[user_id])

update.message.reply_text(response)

  

def main():

updater = Updater(telegram_bot_token)

  

dp = updater.dispatcher

dp.add_handler(CommandHandler("start", start))

dp.add_handler(MessageHandler(Filters.text & ~Filters.command, chat))

  

updater.start_polling()

updater.idle()

  

if __name__ == '__main__':

main()
```

## Run the bot on a test environment

* Create a Google Colab account and login: https://colab.research.google.com/
* Create a "New Notebook"
* One the notebook, run below code:
	* Install OpenAI API, click "run" on the left side.
```Python
!pip install openai 
```
* Install Telegram Bot API, click "run" on the left side.
```Python
 !pip install python-telegram-bot==13.7
```
* Paste the code for your bot, click "run" on the left side

* Open your telegram, go to the bot and start testing it.

#### Limitations
* In Google Colab environment, you can update, edit or change the setting of your bot and push the update easily. It is highly recommended to tune the parametor and role prompt in this testing environment before you deploy to server environment. 
* There are many limitations of running codes on Google colab (remember! it's a testing environment):
	* It will stop once you disconnect from current section.
	* There is a cap on RAM.
	* It will be disruppted when your local connection is unstable.
* Once you confirm this is the bot you want, you can move the code to the virtual machine service, for example, Google Cloud Compute Engine, to make sure your bot will be running without disruption. 

#### Next Step: [[Create a Github Repository]](https://github.com/memonde/how-to-create-chat-bot/blob/main/Create%20a%20Github%20Repository.md)
If you already create a Github Repository, you can go to [[Install and Run Chat Bot on Cloud Compute Engine]](https://github.com/memonde/how-to-create-chat-bot/blob/main/Install%20and%20Run%20Chat%20Bot%20on%20Cloud%20Compute%20Engine.md) for the next step.
