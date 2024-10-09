# 🐚 Bash syntax

## Examples

```shell title=for
for i in {1..99}
do
	echo $i
done
```

```shell title=if
if (( $1 % 2 == 0 ))
then
	echo And is also an even number.
fi
```

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
