# OAUTH_PROJECT

This project demonstrates the integration of social login using Django and [social-auth-app-django](https://github.com/python-social-auth/social-app-django). The project implements authentication via VK and Google using OAuth 2.0.

## Project Structure

```bash
OAUTH_PROJECT/
├── .venv/                   # Virtual environment
├── authapp/                 # Authentication app
│   ├── migrations/          # Database migrations
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   └── views.py             # Main views
├── oauth_project/           # Root folder for Django project settings
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py          # Project configuration (OAuth keys are set here)
│   ├── urls.py              # URL routes
│   └── wsgi.py
├── templates/               # HTML templates
│   ├── login.html           # Login page (with social login links and VKID button if used)
│   └── home.html            # Home page (after authentication)
├── db.sqlite3               # SQLite database
├── manage.py                # Django management script
├── requirements.txt         # Project dependencies
└── README.md                # This file
```

## Installation and Running the Project

1. **Clone the repository:**

   ```bash
   git clone https://github.com/yourusername/OAUTH_PROJECT.git
   cd OAUTH_PROJECT
   ```

2. **Create and activate a virtual environment:**

   ```bash
   python -m venv .venv
   source .venv/bin/activate  # for Linux/macOS
   .\.venv\Scripts\activate   # for Windows
   ```

3. **Install dependencies:**

   ```bash
   pip install -r requirements.txt
   ```

4. **Apply migrations:**

   ```bash
   python manage.py migrate
   ```

5. **Run the server:**

   ```bash
   python manage.py runserver
   ```

## Configuring OAuth for VK and Google

For authentication to work correctly, you must register applications on the respective services and obtain the necessary keys.

### OAuth VK

1. **Register your application:**
   - Go to [VK for Developers](https://dev.vk.com/) and log in.
   - Create a new application in the "My Applications" section. Typically, the "Standalone application" type is used.
   - Specify the **Redirect URI**:
     ```
     https://your-ngrok-domain.ngrok.io/auth/complete/vk-oauth2/
     ```
     (replace `your-ngrok-domain.ngrok.io` with your actual public domain).

2. **Obtain your keys:**
   - **VK_APP_ID**: the application identifier.
   - **VK_APP_SECRET**: the application secret.

3. **Configure variables in `settings.py`:**

   ```python
   # OAuth VK
   SOCIAL_AUTH_VK_OAUTH2_KEY = 'VK_APP_ID'
   SOCIAL_AUTH_VK_OAUTH2_SECRET = 'VK_APP_SECRET'
   SOCIAL_AUTH_VK_OAUTH2_SCOPE = ['offline', 'email', 'friends']
   ```

### OAuth Google

1. **Register your application:**
   - Go to [Google Cloud Console](https://console.cloud.google.com/).
   - Create a new project or choose an existing one.
   - Navigate to **APIs & Services → Credentials** and create credentials of type **OAuth client ID** for a web application.
   - Specify the **Authorized Redirect URIs**:
     ```
     https://your-ngrok-domain.ngrok.io/auth/complete/google-oauth2/
     ```

2. **Obtain your keys:**
   - **GOOGLE_CLIENT_ID**: the client identifier.
   - **GOOGLE_CLIENT_SECRET**: the client secret.

3. **Configure variables in `settings.py`:**

   ```python
   # OAuth Google
   SOCIAL_AUTH_GOOGLE_OAUTH2_KEY = 'GOOGLE_CLIENT_ID'
   SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET = 'GOOGLE_CLIENT_SECRET'
   SOCIAL_AUTH_GOOGLE_OAUTH2_SCOPE = ['offline', 'email', 'friends']
   ```

## Additional Settings

- **ALLOWED_HOSTS:**
  If you are using a public URL (for example, via ngrok), remember to add it to `ALLOWED_HOSTS` in your `settings.py`:

  ```python
  ALLOWED_HOSTS = ['127.0.0.1', 'localhost', 'your-ngrok-domain.ngrok.io']
  ```

- **Ngrok:**
  For development, you can use [ngrok](https://ngrok.com/) to create a public HTTPS tunnel. Example usage:

  ```bash
  ngrok http 127.0.0.1:8000
  ```

  After launching ngrok, obtain the public URL and update it in your VK and Google settings, as well as in the `redirectUrl` parameter in your frontend (if using the VKID button).

## Usage

1. Navigate to the login page at [http://localhost:8000/login/](http://localhost:8000/login/).
2. Choose the authentication method:
   - **VK:** via the link or through the VKID widget (if integrated).
   - **Google:** via the corresponding link.
3. After successful authentication, you will be redirected to the home page.

## Conclusion

The **OAUTH_PROJECT** demonstrates how to set up social login using Django and social-auth-app-django. Registering applications in VK and Google is a mandatory step to obtain valid keys and ensure proper OAuth authentication.

If you have any questions or encounter issues, feel free to reach out!
