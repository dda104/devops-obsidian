# 🐚 Bash syntax

## Examples

For по диапазону чисел:

```shell title=for
for i in {1..99}
do
    ...
done
```

Проверка на четность числа:

```shell title=if
if (( $NUMBER % 2 == 0 ))
then
    ...
fi
```

---

# 🤹‍♀️ Use cases

Получить публичный ip адрес:

```bash
curl -q ifconfig.me
```

Чтение содержимого файла в переменную:

```shell
VARIABLE=$(<file_path)
```

---

# 🥇 Best Practice

Отладка скриптов

```shell
#!/bin/bash
trap 'echo "# $BASH_COMMAND";read' DEBUG
...
```

---

# 🌎 Links

---
