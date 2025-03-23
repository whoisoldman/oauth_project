# OAUTH_PROJECT

Проект демонстрирует интеграцию авторизации через социальные сети с использованием Django и [social-auth-app-django](https://github.com/python-social-auth/social-app-django). В проекте реализована авторизация через VK и Google с использованием OAuth 2.0.

## Графическая структура проекта

```bash
OAUTH_PROJECT/
├── .venv/                   # Виртуальное окружение
├── authapp/                 # Приложение для аутентификации
│   ├── migrations/          # Миграции базы данных
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   └── views.py             # Основные представления (views)
├── oauth_project/           # Корневая папка настроек проекта Django
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py          # Конфигурация проекта (здесь прописаны ключи OAuth)
│   ├── urls.py              # Маршруты URL
│   └── wsgi.py
├── templates/               # Шаблоны HTML
│   ├── login.html           # Страница входа (ссылки для авторизации и кнопка VKID, если используется)
│   └── home.html            # Главная страница (после авторизации)
├── db.sqlite3               # База данных SQLite
├── manage.py                # Скрипт управления Django
├── requirements.txt         # Зависимости проекта
└── README.md                # Этот файл
```

## Установка и запуск проекта

1. **Клонируйте репозиторий:**

   ```bash
   git clone https://github.com/yourusername/OAUTH_PROJECT.git
   cd OAUTH_PROJECT
   ```

2. **Создайте и активируйте виртуальное окружение:**

   ```bash
   python -m venv .venv
   source .venv/bin/activate  # для Linux/macOS
   .\.venv\Scripts\activate   # для Windows
   ```

3. **Установите зависимости:**

   ```bash
   pip install -r requirements.txt
   ```

4. **Примените миграции:**

   ```bash
   python manage.py migrate
   ```

5. **Запустите сервер:**

   ```bash
   python manage.py runserver
   ```

## Настройка OAuth для VK и Google

Для корректной работы авторизации необходимо зарегистрировать приложения в соответствующих сервисах и получить необходимые ключи.

### OAuth VK

1. **Регистрация приложения:**
   - Перейдите на [VK для разработчиков](https://dev.vk.com/) и авторизуйтесь.
   - Создайте новое приложение в разделе "Мои приложения". Обычно используется тип "Standalone приложение".
   - Укажите **Redirect URI**:
     ```
     https://your-ngrok-domain.ngrok.io/auth/complete/vk-oauth2/
     ```
     (замените `your-ngrok-domain.ngrok.io` на ваш актуальный публичный домен).

2. **Получите ключи:**
   - **VK_APP_ID**: идентификатор приложения.
   - **VK_APP_SECRET**: секретное значение приложения.

3. **Настройте переменные в `settings.py`:**

   ```python
   # OAuth VK
   SOCIAL_AUTH_VK_OAUTH2_KEY = 'VK_APP_ID'
   SOCIAL_AUTH_VK_OAUTH2_SECRET = 'VK_APP_SECRET'
   SOCIAL_AUTH_VK_OAUTH2_SCOPE = ['offline', 'email', 'friends']
   ```

### OAuth Google

1. **Регистрация приложения:**
   - Перейдите в [Google Cloud Console](https://console.cloud.google.com/).
   - Создайте новый проект или выберите существующий.
   - Перейдите в раздел **APIs & Services → Credentials** и создайте учетные данные типа **OAuth client ID** для веб-приложения.
   - Укажите **Authorized Redirect URIs**:
     ```
     https://your-ngrok-domain.ngrok.io/auth/complete/google-oauth2/
     ```

2. **Получите ключи:**
   - **GOOGLE_CLIENT_ID**: идентификатор клиента.
   - **GOOGLE_CLIENT_SECRET**: секрет клиента.

3. **Настройте переменные в `settings.py`:**

   ```python
   # OAuth Google
   SOCIAL_AUTH_GOOGLE_OAUTH2_KEY = 'GOOGLE_CLIENT_ID'
   SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET = 'GOOGLE_CLIENT_SECRET'
   SOCIAL_AUTH_GOOGLE_OAUTH2_SCOPE = ['offline', 'email', 'friends']
   ```

## Дополнительные настройки

- **ALLOWED_HOSTS:**
  Если вы используете публичный URL (например, через ngrok), не забудьте добавить его в `ALLOWED_HOSTS` в файле `settings.py`:

  ```python
  ALLOWED_HOSTS = ['127.0.0.1', 'localhost', 'your-ngrok-domain.ngrok.io']
  ```

- **Ngrok:**
  Для разработки можно использовать [ngrok](https://ngrok.com/) для создания публичного https-туннеля. Пример запуска:

  ```bash
  ngrok http 127.0.0.1:8000
  ```

  После запуска получите публичный URL и обновите его в настройках VK и Google, а также в параметре `redirectUrl` в вашем фронтенде (если используется кнопка VKID).

## Использование

1. Перейдите на страницу входа по адресу [http://localhost:8000/login/](http://localhost:8000/login/).
2. Выберите способ авторизации:
   - **VK:** перейдя по ссылке или через виджет кнопки (если интегрирован VKID).
   - **Google:** перейдя по соответствующей ссылке.
3. После успешной авторизации вы будете перенаправлены на главную страницу.

## Заключение

Проект **OAUTH_PROJECT** демонстрирует, как можно настроить авторизацию через социальные сети с использованием Django и social-auth-app-django. Регистрация приложений в VK и Google является обязательным шагом для получения валидных ключей и корректной работы OAuth авторизации.

Если возникнут вопросы или проблемы — обращайтесь!
