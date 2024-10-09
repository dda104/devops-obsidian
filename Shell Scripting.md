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

Чтение содержимого файла в переменную:

```shell
VARIABLE=$(<file_path)
```

---

# 🤹‍♀️ Use cases

Получить публичный ip адрес:

```bash
curl -q ifconfig.me
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
