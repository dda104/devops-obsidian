# 🐚 Bash Specific

for

```shell
for i in {1..99}
do
	echo $i
done
```

if

```shell
if (( $1 % 2 == 0 ))
then
	echo And is also an even number.
fi
```

Отладка скриптов

```shell
#!/bin/bash
trap 'echo "# $BASH_COMMAND";read' DEBUG
...
```

---

# 🤹‍♀️ Use cases

Получить публичный ip адрес:

```bash
curl -q ifconfig.me
```

---

# 🥇 Best Practice

---

# 🌎 Links

---
