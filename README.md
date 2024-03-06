
import telebot
from telebot import types
import requests

API_KEY = "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"
BOT_TOKEN = "6573158663:AAElXYSII-NZt1s1OCzPJ0Dm_KFhrPnJcLc"

bot = telebot.TeleBot(BOT_TOKEN)


@bot.message_handler(commands=['start'])
def start(message):
    bot.reply_to(message,"🤝")
    markup = types.InlineKeyboardMarkup()
    btn = types.InlineKeyboardButton(text='GPT4 TURBO UPDATES', url='https://t.me/GPT4_TURBO')
    markup.add(btn)
    bot.send_message(message.chat.id, f"Hello : <a href='tg://user?id={message.from_user.id}'>{message.from_user.first_name}</a>\nI am a bot made with <b>GPT-4 </b>Artificial Intelligence. \nIf you ask me a question, I will answer you quickly. Before you ask, click on the button and  Join my channel.",parse_mode="HTML", reply_markup=markup)


def get_response(message):
    url = "https://api.openai.com/v1/chat/completions"
    headers = {
        "Authorization": f"Bearer {API_KEY}"
    }
    data = {
        "model": "gpt-3.5-turbo",
        "messages": [
            {
                "role": "user",
                "content": message
            }
        ]
    }
    response = requests.post(url, headers=headers, json=data)
    return response.json()["choices"][0]["message"]["content"]

@bot.message_handler(func=lambda message: True)
def reply_to_message(message):
    markup = types.InlineKeyboardMarkup()
    btn = types.InlineKeyboardButton(text='📣 𝙂𝙋𝙏-4 𝗕𝗼𝘁 𝗨𝗽𝗱𝗮𝘁𝗲𝘀', url='https://t.me/GPT4_TURBO')
    markup.add(btn)
    chat_id = message.chat.id
    message_text = message.text
    test = get_response(message_text)
    bot.send_message(chat_id, "Hello"+test+"\n\n*🤖 @GPT4TURB0BOT*",parse_mode="Markdown",reply_markup=markup, reply_to_message_id=message.message_id)

bot.polling()