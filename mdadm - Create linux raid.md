# 📖 Introduction

**Mdadm** - Утилита linux для управления raid массивами

---

# 👨‍🏭 Usage

## 👀 Check Status

Проверить статус существующих raid массивов

```shell
cat /proc/mdstat
```

Полезно смотреть состояние дисков через `lsblk`

> [!NOTE]
>  Raid массивы имеют названия формата md*n*, например: md0

## 🔨 Create Raid 1

Создание нового raid1 массива из свободных устройств `/dev/sda` и `/dev/sdb`

```shell
mdadm --create --verbose /dev/*Raid_device_name* --level=1 --raid-devices=2 /dev/*Disk_devices*
```

>[!NOTE]
> `Raid_device_name` - Это название нового девайса например: `md1`
> 
> `Disk_devices` - Это существующие устройства из которых будет собран raid например: `sd[a-b]`

## 🚀 Automount via fstab

Для того чтобы обеспечить монтирование диска в fs требуется добавить запись в fstab.
Для этого требуется получить ID устройства:

```shell
blkid
```

После добавить монтирование по существующим примером с требуемым ID в `/etc/fstab`

Для проверки можно выполнить:

```shell
mount /*Path_mount_point*
```

>[!NOTE]
> `Path_mount_point` - Это путь до точки монтирования

Также можно выполнить перезагрузку с проверкой `df -h`

---

# 🌍 Links

- [Mdadm xgu.ru](http://xgu.ru/wiki/mdadm)

---
