# Вопросы по Docker

## Отличия ADD от COPY

ADD Может работать с ссылками и архивами(распаковывать)

```dockerfile
ADD https://example.com/file.tar.gz /usr/src/app/
ADD my-archive.tar.gz /usr/src/app/
```

## Expose


