# GnuPG (GNU Privacy Guard)


## Начало работы с ключами


### Конфиг GnuPG

/.gnupg/gpg.conf

```
keyid-format 0xlong
throw-keyids
no-emit-version
no-comments
```

### Генерация и отображение

`gpg --full-gen-key` - генарация ключей

`gpg -k` - отображение публичных ключей 

`gpg -K` - отображение приватных ключей

## Шифрование и дешифрование файлов

(`-a` - ASCII формат, `your-id-key` - указание ключа шифрования)

`gpg -e -a -r your-id-key file.txt` - шифрование файла

`gpg -d -o file.txt file.txt.asc` - дешифрование файла

## Экспорт и импорт ключей

### Экспорт

`gpg --export -a your-id-key > public.gpg` - экспорт публичных ключей

`gpg --export-secret-key -a your-id-key > secret.gpg` - экспорт приватных ключей

### Удаление

`gpg --delete-secret-keys your-id-key` - удаление приватных ключей 

`gpg --delete-keys your-id-key` - удаление публичных ключей

### Импорт

`gpg --import public.gpg` - импорт публичных ключей

`gpg --import private.gpg` - импорт приватных ключей
