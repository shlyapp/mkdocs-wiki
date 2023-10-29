# Использование нескольких gpg ключей для авторизации на разных сайтах

Пример github/gitlab <br />

Файл `~/.ssh/config`
```
# Конфигурация для GitHub
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa

# Конфигурация для GitLab
Host gitlab.com
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_rsa_work
```

Так же стоит понимать, что, если ключи импотированы, то надо изменить им права доступа:
```shell
chmod 600 id_rsa_work
```


