# 🤖 Create bot

Для создания бота перейти в [BotFather](https://t.me/BotFather), выполнить `/newbot` и следовать инструкциям.

---

# 💬 Get Chat_id

1. Добавить бота в чат
2. В чате выполнить команду `/start`, на этом этапе никакого вывода быть не должно
3. Открыть в браузере страницу `https://api.telegram.org/bot<Token>/getUpdates` и найти в json `"chat":{"id":<Chat_ID>,...`

>[!NOTE] 
>`Token` - Это токен бота который можно получить в [BotFather](https://t.me/BotFather)
>
>`Chat_ID` - Это Chat ID

>[!TIP] Для получения можно использовать скрипт из 4 пункта Get [[Telegram Bot#Get Msg_thread_id|Get Msg_thread_id]]

---

# ✉️ Get Msg_thread_id

Если в чате есть топики то может потребоваться знание `Msg_thread_id` для того чтобы бот мог писать в определенный топик

>[!NOTE] 
>Без указания `Msg_thread_id` Сообщения будут попадать в топик `General`

1. Добавить бота в чат с топиками
2. Опционально создать virtualenv:    

```shell
python -m venv .venv
source .venv/bin/activate
```

3. Установить telebot:

```shell
pip install pyTelegramBotAPI
```

4. Создать и запустить python скрипт

```python title=main.py
import telebot

TOKEN = '<Token>'

bot = telebot.TeleBot(TOKEN)

@bot.message_handler(func=lambda message: True)
def echo_message(message):
    chat_id = message.chat.id
    try:
        msg_thread_id = message.reply_to_message.message_thread_id
    except AttributeError:
        msg_thread_id = "General"
    bot.reply_to(message, f"Chat ID этого чата: {chat_id}\nИ message_thread_id: {msg_thread_id}")

bot.polling()
```

>[!NOTE] 
>`Token` - Это токен бота который можно получить в [BotFather](https://t.me/BotFather)

5. Выполнить в требуемом топике `/start`

>[!NOTE] 
>Должен появиться ответ на сообщение вида:
>Chat ID этого чата: <Chat_id>
И message_thread_id: <Msg_thread_id>

---

# 🌎 Links

- [BotFather](https://t.me/BotFather)
- [PyPi Telebot](https://pypi.org/project/pyTelegramBotAPI/)
- [PyPi Virtualenv](https://pypi.org/project/virtualenv/)

---
