# 🦙 Ollama

>[!INFO] LM Studio показывает себя стабильнее в работе

## ⬇️ Install on linux

```shell
curl -fsSL https://ollama.com/install.sh | sh
```

## 🤖 Get LLM

```shell
ollama pull *Model_name*
```

>[!INFO] `Model_name` - Это название модели, доступные можно найти на [Ollama Library](https://ollama.com/library)

## 🏃 Run LLM

```shell
ollama pull *Model_name*
```

>[!INFO] `Model_name` - Это название модели, доступные можно найти на [Ollama Library](https://ollama.com/library)

---

# 📦 LM Studio

## ⬇️ Install

Скачать пакет под свою систему на [LM Studio Website](https://lmstudio.ai)

## 🤖 Get LLM

Во вкладке Discover можно найти и установить требуемую модель

## 🏃 Run LLM

Для запуска перейти во вкладку Developer, Нажать Run Server, Выбрать модель в Select model to load

---

# ⏩ Continue plugin for IDE

Установить Continue плагин для vscode или jetbrains

Перейти в конфигурацию плагина

## 🦙 Ollama configuration

Пример конфигурации:

```json
{
  "models": [
    {
      "apiBase": "http://localhost:11434/",
      "model": "codellama",
      "provider": "ollama",
      "title": "CodeLlama"
    }
  ],
  "tabAutocompleteModel": {
    "title": "Starcoder2 3b",
    "provider": "ollama",
    "model": "starcoder2:3b"
  },
  "tabAutocompleteOptions": {
    "debounceDelay": 500,
    "maxPromptTokens": 100
  },
...
```

## 📦 LM Studio configuration

Пример конфигурации:

```json
{
  "models": [
    {
      "apiBase": "http://localhost:1234/v1",
      "model": "autodetect",
      "provider": "lmstudio",
      "title": "codestral"
    }
  ],
  "tabAutocompleteModel": {
    "title": "codestral",
    "provider": "lmstudio",
    "model": "autodetect",
    "apiBase": "http://localhost:1234/v1"
  },
  "tabAutocompleteOptions": {
    "debounceDelay": 500,
    "maxPromptTokens": 100
  },
  ...
```

---

# 🌎 Links

- [Ollama website](https://ollama.com)
- [Ollama Library](https://ollama.com/library)
- [LM Studio Website](https://lmstudio.ai)
- [Continue plugin](https://marketplace.visualstudio.com/items?itemName=Continue.continue)
- [Add ollama in vscode guide by dev.to](https://dev.to/manjushsh/configuring-ollama-and-continue-vs-code-extension-for-local-coding-assistant-48li)

---
