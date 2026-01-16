
WARNING!
If you do not back up the LUKS header, you may permanently lose access to all encrypted data in the event of any failure or accidental damage.

What happens if the LUKS header is damaged?

All your data will become INACCEPTABLE.
Even if you know the correct password, you will not be able to decrypt the disk.

No recovery software will help - without the header, the LUKS volume turns into a useless set of bytes.

Why do you MUST do this NOW?
One disk failure, random error or metadata corruption - your data will be lost forever.
Entering the "kill" password perfectcrypt WILL DESTROY HEADER!

WARNING: IF YOU IGNORE THIS MESSAGE, YOU KNOWINGLY RISK LOSS OF ALL DATA.
THE PACKAGE DEVELOPERS WILL NOT BE ABLE TO HELP YOU RECOVER YOUR DATA!

Here's the command to use (replace `<device>` with the path of the device
file representing your luks encrypted partition, and `<your-backup-file>`
with the path to the backup file to create):
```
$ sudo cryptsetup luksHeaderBackup <device> --header-backup-file <your-backup-file>
```

To later restore the header, you will have to do:
```
$ sudo cryptsetup luksHeaderRestore <device> --header-backup-file <your-backup-file>
```

========================================================================================

ВНИМАНИЕ!
Если вы не создадите резервную копию заголовка LUKS, вы можете безвозвратно потерять доступ ко всем зашифрованным данным при любом сбое или случайном повреждении.

Что произойдет, если заголовок LUKS будет поврежден?

Все ваши данные станут НЕДОСТУПНЫ.
Даже зная правильный пароль, вы не сможете расшифровать диск.

Никакие программы для восстановления не помогут — без заголовка том LUKS превращается в бесполезный набор байт.

Почему это НЕОБХОДИМО сделать СЕЙЧАС?
Один сбой диска, случайная ошибка или повреждение метаданных — данные будут утеряны навсегда.
Ввод "смертельного" пароля perfectcrypt УНИЧТОЖИТ ЗАГОЛОВОК!

ПРЕДУПРЕЖДЕНИЕ: ЕСЛИ ВЫ ПРОИГНОРИРУЕТЕ ЭТО СООБЩЕНИЕ, ВЫ ОСОЗНАННО ИДЁТЕ НА РИСК ПОТЕРИ ВСЕХ ДАННЫХ.
РАЗРАБОТЧИКИ ПАКЕТА НЕ СМОГУТ ПОМОЧЬ ВАМ ВОССТАНОВИТЬ ДАННЫЕ!

Каждый запуск системы без резервной копии — игра в русскую рулетку с вашими данными.

Вот команда, которую нужно использовать (замените `<device>` на путь к файлу устройства, представляющему ваш зашифрованный раздел Luks, а `<your-backup-file>`
на путь к файлу резервной копии, который нужно создать):
```
$ sudo cryptsetup luksHeaderBackup <device> --header-backup-file <your-backup-file>
```

Чтобы позже восстановить заголовок, выполните:
```
$ sudo cryptsetup luksHeaderRestore <device> --header-backup-file <your-backup-file>
