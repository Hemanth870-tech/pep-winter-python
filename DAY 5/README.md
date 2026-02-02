# DAY 5 WORK
1. Install the django-tailwind package via pip:
Install with all dependencies that make development easier:

python -m pip install 'django-tailwind[cookiecutter,honcho,reload]'

This includes:
cookiecutter - for creating theme apps
honcho - for running Django and Tailwind servers simultaneously with python manage.py tailwind dev
django-browser-reload - for automatic page reloads during development.

2. Add 'tailwind' to INSTALLED_APPS in settings.py:

INSTALLED_APPS = [
    # other Django apps
    "tailwind",]


3. Create a Tailwind CSS compatible Django app, I like to call it thor:

python manage.py tailwind init

You will need to provide the name of the app and choose the installation method: standalone binary or npm-based.
See Standalone vs npm-based for a detailed comparison.

4. Add your newly created 'thor' app to INSTALLED_APPS in settings.py:

INSTALLED_APPS = [
    # other Django apps
    "tailwind",
    "thor",]

5. Register the generated 'thor' app by adding the following line to settings.py:

TAILWIND_APP_NAME = "thor"

6. Install Tailwind CSS dependencies by running the following command:

python manage.py tailwind install

7. The Django Tailwind comes with a simple base.html template located at your_tailwind_app_name/templates/base.html.

8. Let’s also add and configure django_browser_reload, which takes care of automatic page and CSS refreshes in development mode. Add it to INSTALLED_APPS in settings.py:

INSTALLED_APPS = [
    # other Django apps
    "tailwind",
    "thor",]

if DEBUG:
    # Add django_browser_reload only in DEBUG mode
    INSTALLED_APPS += ["django_browser_reload"]

TAILWIND_APP_NAME = "thor"
NPM_BIN_PATH = r"C:\Program Files\nodejs\npm.cmd"