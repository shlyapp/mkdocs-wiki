# Как подключить Bootstrap к своему проекту (через static файлы)

## Скачиваем исходники bootstrap

Переходим по [ссылке](https://getbootstrap.com/docs/5.2/getting-started/download/) и скачиваем архив. </br>
Далее создаем в проекте папку `static`, в нее распаковываем архив `bootstrap`

## Подключение static файлов

Переходим в настройки проекта django `settings.py` и добавляем следующие строки:
```python
import os
STATIC_URL = 'static/'
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]
```

## Использование в шаблонах

Для того, чтобы можно было использовать в шаблонах bootstrap, в .html надо добавить следующее:

```html
{% load static %}
<link href="{% static 'bootstrap/css/bootstrap.min.css' %}" rel="stylesheet">
<script src="{% static 'bootstrap/js/bootstrap.min.js' %}"></script>
```

## Пример использования

```html
<button type="button" class="btn btn-success">Success</button>
```



