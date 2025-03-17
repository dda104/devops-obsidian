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
- *command 2* - Команда ожидающая в качестве аргумента каждый по отдельности элемент из первой

Пример использования:

В текущей директории существует файл `.tool-versions` из которого необходимо добавить все плагины

```shell title=.tool-versions
python 3.13.2
direnv 2.35.0
task 3.41.0
goss 0.4.9
```

Добавить все плагины можно командой:

```shell
cat .tool-versions | awk '{print $1}' | xargs -i asdf plugin add {}
```

- `cat .tool-versions` - выведет файл
- `awk '{print $1}'` - выведет только первый столбец: `python`, `direnv`, `task`, `goss`
- `xargs -i asdf plugin add {}` - составит команды с данными аргументами и добавит плагины, то есть эта команда заменяет следующие 4:

```shell
asdf plugin add python
asdf plugin add direnv
asdf plugin add task
asdf plugin add goss
```

---

# 📄 Sed

Заменить все вхождения:

```shell
<command> | sed -e 's/<delete words>/<add words>/g'
```

> [!note]
> м

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
