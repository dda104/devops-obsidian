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

# Xargs

Xargs - Это инструмент способный в простых случаях заменить for, пример использования:

```shell
<command 1> xargs -i <command 2> {}
```

- *command 1* - Команда возвращаемая массив элементов которые следует использовать в цикле
- 

---

# 📄 Sed

Вывести 10 строку из file

```shell
sed -n '10p' file
```

---

# 📄 Awk

Вывести первый столбец

```shell
<command> | awk '{print $1}'
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

Для корректной остановки скрипта после 1 ошибочно выполненной команды:

```shell
#!/bin/bash
set -euxo pipefail
...
```

Отладка скриптов:

```shell
#!/bin/bash
trap 'echo "# $BASH_COMMAND";read' DEBUG
...
```

## Shellcheck

Для линтинга shell скриптов рекомендуется использовать shellcheck или другие линтеры

---

# 🌎 Links

- [sed gnu.org](https://www.gnu.org/software/sed/manual/sed.html)
- [awk Website](http://www.awklang.org)

---
