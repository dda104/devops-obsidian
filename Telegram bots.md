# 🤖 Create bot

Для создания бота перейти в [BotFather](https://t.me/BotFather), выполнить `/newbot` и следовать инструкциям.

---

# 💬 Get Chat_id

1. Добавить бота в чат
2. В чате выполнить команду `/start`, на этом этапе никакого вывода быть не должно
3. Открыть в браузере страницу `https://api.telegram.org/bot*Token*/getUpdates` и найти в json `"chat":{"id":*ID*,...`

>[!INFO] `*Token*` - Это токен бота который можно получить в [BotFather](https://t.me/BotFather)

> [!INFO] `ID` - Это Chat ID

---

# ✉️ Msg_thread_id

Если в чате есть топики то может потребоваться знание `Msg_thread_id` для того чтобы бот мог писать в определенный топик

>[!INFO] Без указания `Msg_thread_id` Сообщения будут попадать в топик `General`

---

# 🌎 Links

[BotFather](https://t.me/BotFather)

---
