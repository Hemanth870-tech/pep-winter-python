# DAY 6 WORK
### git commands:

1.	Git init              – Used to create a .git folder (master) for our project.
2.	 Touch and Nano       – Used to create and edit the files .
3.	Git add .             – Used to add every file into the git staging area which is an intermediate space for review changes before commiting history to project repository.
4.	Git commit -m         – Used to save the snapshots of your file into your local repository.
5.	Git log –online       – Used to see the commit history
6.	Git remote add origin – connects your .git folder to Github.
7.	Git branch -m main    – renames your branch from master to main
8.	Git push -u origin main – used to push all your files to Github.
9.  Git status            - used to check that which files are tracked and utracked in git repository.
10. Git branch <branch-name> - used to create a new branch for new features.
11. Git checkout <branch-name> - switch to the branch
12. Git checkout -b <branch-name> - directly creating and switching to new branch.
13. Git checkout -D <brnach-name> - delete a branch

Created a pull requestes and merged the branches to main.
When there is a merge conflict which means someone has pushed the changes before you which are not with you so yu need to pull the changes and then push your changes.

14. Git pull - to pull all the changes that we need to have.

After a new collaborator is added you will clone the repo:
15. git clone <your-repo-link>
16. To push you need to use word remote = git push remote <branch-name>

After inviting a collaborator to do changes he used git clone and pushed it.


### TAILWIND CONTINUE:
1. Let’s also add and configure django_browser_reload, which takes care of automatic page and CSS refreshes in development mode. Add it to INSTALLED_APPS in settings.py:

INSTALLED_APPS = [
    # other Django apps
    "tailwind",
    "theme",
]
if DEBUG:
    # Add django_browser_reload only in DEBUG mode
    INSTALLED_APPS += ["django_browser_reload"]

2. Staying in settings.py, add the middleware:

if DEBUG:
    # Add django_browser_reload middleware only in DEBUG mode
    MIDDLEWARE += [
        "django_browser_reload.middleware.BrowserReloadMiddleware",
    ]


The middleware should be listed after any that encode the response, such as Django’s GZipMiddleware. The middleware automatically inserts the required script tag on HTML responses before </body> when DEBUG is True.

3. Include django_browser_reload URL in your root urls.py:

from django.urls import include, path
from django.conf import settings
urlpatterns = [
    # other URL patterns
]
if settings.DEBUG:
    # Include django_browser_reload URLs only in DEBUG mode
    urlpatterns += [
        path("__reload__/", include("django_browser_reload.urls")),
    ]


Finally, you should be able to use Tailwind CSS classes in HTML. You have two options to start development:

Option 1 (Recommended): Start both Django and Tailwind development servers simultaneously:

python manage.py tailwind dev

Option 2: Start only the Tailwind watcher (you’ll need to run python manage.py runserver separately):

python manage.py tailwind start

Check out the Usage section for more details and information about the production mode.