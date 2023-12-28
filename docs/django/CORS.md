# Как исправить ошибку CORS Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at

## Когда возникает эта ошибка?

Данная ошибка возникает когда на backend приходит запрос c неизвестного ip адреса. Политика CORS не позволяет обработать данный запрос.

## Как исправить?

Для того, что исправить данную проблему надо внести несколько изменений в backend проекта (`settings.py`):

- `pip install django-cors-headers`
- добавляем строку `corsheaders` в `INSTALLED_APPS`
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'corsheaders',
]
```
- добавляем строку в `MIDDLEWARE`
```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

- добавляем `CORS_ORIGIN_WHITELIST`
```
CORS_ORIGIN_WHITELIST = [
    "http://localhost:8080",
    "http://127.0.0.1:8000"
]
```
