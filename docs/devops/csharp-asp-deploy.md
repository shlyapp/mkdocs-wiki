# Как задеплоить проект C# ASP.NET

## Подготовка

### Подключение по ssh к удаленному серверу

Чтобы подключиться к удаленному серверу по ssh надо ввести следующую команду:
```sh
ssh root@192.192.192.192
```
`root` - пользователь, под который мы заходим </br>
`192.192.192.192` - адрес сервера, к которому подклюаемся </br>
Если надо указать конкретно порт, то добавляем ключ `-p 8000` с указанием нужного порта </br>

Чтобы войти на сервер потребуется ввести пароль от пользователя, под которым осущетсвляется вход или использовать ключи доступа.

### Обновление и установка пакетов

Обновление пакетов:
```sh
sudo apt update && apt upgrade
```
Установка нужных пакетов:
```sh
sudo apt install vim tmux tree nginx
```
`vim` - удобный текстовый редактор </br>
`tmux` - терминальный мультиплексор (разбирает терминал на части) </br>
`tree` - просмотр дерева файлов </br>
`nginx` - наш web server </br>

### Установка .NET 

#### Скачиваем и устанавливаем

Следуем инструкции на [официальном сайте Microsoft](https://learn.microsoft.com/en-us/dotnet/core/install/linux-scripted-manual#scripted-install): </br>

- Скачиваем скрипт автоматической установкой
```sh
wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
```
- Выдаем права на запуск
```sh
chmod +x ./dotnet-install.sh
```
- Устанавливаем SDK
```sh
./dotnet-install.sh --version latest
```
- Устанавливаем ASP.NET
```sh
./dotnet-install.sh --version latest --runtime aspnetcore
```
- Устанавливаем нужную версию .NET (6.0 меняем на свою)
```sh 
./dotnet-install.sh --channel 6.0
```

#### Переменные окружения для запуска

Чтобы воспользоваться командой `dotnet` необходимо установить переменные окружения: </br>
```sh
export DOTNET_ROOT=$HOME/.dotnet
```
```sh
export PATH=$PATH:$DOTNET_ROOT:$DOTNET_ROOT/tools
```

#### Проверяем, все ли корректно установилось

Проверяем установку следующей командой:
```sh
dotnet --version
```
Если появилось информация о версии .NET, то все установилось корректно, в противном случае может быть ошибка `dotnet: command not found`, это означает, что система не знает данную комманду.

## Запуск проекта

### Копирование проекта

Чтобы скопировать проект с github, надо воспользоваться коммандой (указываем ссылку на свой репозиторий):
```sh
git clone https://github.com/shlyapp/mkdocs-wiki.git
```

### Запуск проекта

Переходим в директорию с проектом при помощи команды 
```sh
cd app/
```

Запускаем проект при помощи команды:
```sh
dotnet run
```

### Как проверить?

Чтобы проверить, все ли корректно запустилось и наш проект работает, воспользуемся утилитой curl для отправки запросов (где укажем локальный хост, порт, на котором работает приложение и нужный запрос):
```
curl http://localhost:5000/
```

Если нам выводится ответ от севера, который мы ожидаем, то все работает исправно и проект запустился локально.

### Создаем публикацию проекта

Находясь в папке с проектом запускаем комманду для публикации проекта:
```sh
dotnet publish
```

Теперь в папке `app/bin/Debug/net6.0/publish/` есть файл `App.dll`, который будем в дальнейшем запускать.

## Создание службы

Созданим службу для автоматического запуска нашего проекта.</br>

- создаем файл и пишем конфиг:
```sh
vim /etc/systemd/system/kestrel-deploy-asp.service
```
```sh
[Unit]
Description=My .NET Web API App

[Service]
WorkingDirectory=/root/app/ 
ExecStart=/root/.dotnet/dotnet /root/app/bin/Debug/net6.0/ProductApi.dll
Restart=always

[Install]
WantedBy=multi-user.target
```

- включаем сервис:
```sh
sudo systemctl enable kestrel-deploy-asp.service
```
- запускаем:
```sh
sudo systemctl start kestrel-deploy-asp.service
```

- проверяем статус:
```sh
sudo systemctl status kestrel-deploy-asp.service
```

## Подключение к Nginx

### Открытие портов

- скачиваем ufw (если нет):
```sh
sudo apt install ufw
```

- открываем порты:
```sh
sudo ufw allow 'Nginx Full'
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443
```

- запускаем:
```sh
sudo ufw enable
sudo ufw status
```

### Настраиваем конфиг

- создаем файл и пишем конфиг:
```sh
vim /etc/nginx/sitec-available/you-server-name.ru
```
```sh
server { 
    listen 80; 
    server_name you-server-name.ru; 
    location / 
    { 
        proxy_pass http://127.0.0.1:5000; 
        proxy_http_version 1.1; 
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme; 
    }
}
```

- создаем ссылку:
```sh
sudo ln -s /etc/nginx/sites-available/you-server-name.ru /etc/nginx/sites-enabled/
```

- редактируем конфиг:
```sh
sudo vim /etc/nginx/nginx.conf
```

- расскомментируем строку `server_names_hash_bucket_size 64` в блоке `http`

### Запуск 

- запукаем!
```sh
sudo systemctl restart nginx
sudo systemctl restart kestrel-deploy-asp
```
