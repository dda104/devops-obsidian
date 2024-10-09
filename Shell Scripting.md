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

# 📄 Sed

Вывести 10 строку из file

```shell
sed -n '10p' file
```

---

# 📄 Awk

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

Для корректной остановки с

Отладка скриптов

```shell
#!/bin/bash
trap 'echo "# $BASH_COMMAND";read' DEBUG
...
```

---

# 🌎 Links

- [sed gnu.org](https://www.gnu.org/software/sed/manual/sed.html)
- [awk Website]((http://www.awklang.org)

---
