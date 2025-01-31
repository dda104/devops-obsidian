# Вопросы по Docker

## Отличия ADD от COPY

ADD Может работать с ссылками и архивами(распаковывать)

```dockerfile
ADD https://example.com/file.tar.gz /usr/src/app/
ADD my-archive.tar.gz /usr/src/app/
```

## Expose

Служит для описания портов для открытия

```dockerfile
EXPOSE 8080
```

Порт сам не откроется, expose нужен для документации, является хорошим тоном и при использовании ключа `-P` откроются по