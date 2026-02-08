# DAY 10 WORK
### Today we have completed the use of Models, Views, Template where we have redirected three pages which are home(student registration), login, signup.
1. polls/models.py classes .

from django.contrib.auth.hashers import identify_hasher, make_password, check_password
class LoginUser(models.Model):
    """Simple login user model stored seperately from Django's auth user."""
    
    username = models.CharField(max_length=150, unique=True)
    password = models.CharField(max_length=128)
    is_active = models.BooleanField(default=True)
    created_at = models.DateTimeField(auto_now_add=True)
    
    def set_password(self, raw_password: str) -> None:
        """Hash and set a raw password."""
        self.password = make_password(raw_password)
    
    def check_password(self, raw_password: str) -> bool:
        return check_password(raw_password,self.password)
    
    def save(self, *args, **kwargs):
        #Ensure stored password is always hased once
        try:
            identify_hasher(self.password)
        except ValueError:
            self.password = make_password(self.password)
        super().save(*args, **kwargs)
    
    def __str__(self):
        return self.username

class SignupUser(models.Model):
    """User model populated via the signup page."""
    
    username = models.CharField(max_length=150, unique=True)
    email = models.EmailField(unique=True)
    password = models.CharField(max_length=128)
    is_active = models.BooleanField(default=True)
    created_at = models.DateTimeField(auto_now_add=True)
    
    def set_password(self, raw_password: str) -> None:
        self.password = make_password(raw_password)
        
    def check_password(self, raw_password: str) -> bool:
        return check_password(raw_password, self.password)
    
    def save(self, *args, **kwargs):
        try:
            identify_hasher(self.password)
        except ValueError:
            self.password = make_password(self.password)
        
        super().save(*args, **kwargs)
        #keep LoginUser in sync so login view works without extra logic
        login_user, _ = LoginUser.objects.get_or_create(username=self.username)
        login_user.password = self.password #already hashed above
        login_user.is_active = self.is_active
        login_user.save()
    
    def __str__(self):
        return self.username

2. polls/views.py

from .models import users, LoginUser, SignupUser
from django.contrib import messages
from django.shortcuts import redirect
def login_view(request):
    if request.method == "POST":
        username = request.POST.get("username")
        password = request.POST.get("password")
        
        user = None
        try:
            user = SignupUser.objects.get(username=username, is_active=True)
        except SignupUser.DoesNotExist:
            try:
                user = LoginUser.objects.get(username=username, is_active=True)
            except LoginUser.DoesNotExist:
                user = None
                
        if user and user.check_password(password):
            messages.success(request, f"Welcome, {username}!")
            return redirect("home/")
        
        messages.error(request, "invlaid username and password")
    
    return render(request, "login.html")


def signup_view(request):
    if request.method == "POST":
        username = request.POST.get("username")
        email = request.POST.get("email")
        password = request.POST.get("password")
        
        if not username or not email or not password:
            messages.error(request, "all fields should be required")
        elif SignupUser.objects.filter(username=username).exists():
            messages.error(request,"username is already taken")
        elif SignupUser.objects.filter(email=email).exists():
            messages.error(request, "email is already registered.")
        else:
            new_user = SignupUser(username=username, email=email)
            new_user.set_password(password)
            new_user.save()
            messages.success(request, "signup successful. you can now log in.")
            return redirect("login")
    
    return render(request, "signup.html")
In this code you can see that signup will redirect to login page and login page will direct to home.html page.

3. polls/urls.py

from .views import home_view, form_view, signup_view
urlpatterns = [
    # path("admin/", admin.site.urls),
    path('',views.index,name = "index"),
    path('home/', views.home_view, name="home_view"),
    path("form",views.form_view, name='form_view'),
    path("login",views.login_view,name="login"),
    path("signup",views.signup_view,name="signup")
]

4. polls/admin.py we need to register our classes in admin which work like a database storage.

admin.site.register(LoginUser)
admin.site.register(SignupUser)

5. polls/template/login.html

<!DOCTYPE html>
<html>
    <head>
        <title> Login </title>
    </head>
    <body>
        <h2>Login</h2>
        {% if messages %}
        <ul>
            {% for message in messages %}
                <li>{{ message }}</li>
            {% endfor %}
        </ul>
        {% endif %}
        <form method = "post" action="">
            {% csrf_token %}
            <label for="username">Username:</label>
            <input type="text" id="username" name="username" required>
            <br>
            <label for="password">Password:</label>
            <input type="password" id="password" name="password" required>
            <br>
            <button type="submit">Login</button>
        </form>
    </body>
    </html>

6. polls/template/signup.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Login</title>
</head>
<body>
    <h2>SignUp</h2>
    {% if messages %}
    <ul>
        {% for message in messages %}
            <li>{{ message }}</li>
        {% endfor %}
    </ul>
{% endif %}
<form method="post" action="">
    {% csrf_token %}
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required>
    <br>
    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required>
    <br>
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>
    <br>
    <button type="submit">SignUp</button>
</form>
</body>
</html>

7. -> python manage.py migrate
8. -> python manage.py makemigrations
9. -> python manage.py migrate
10. -> python manage.py runserver

### The study that i ahve done upon these things are:
What is Django?
Django is a complete web development framework built with Python. It provides everything needed to create web applications quickly without dealing with low-level details. Think of it as a toolbox with pre-built components for common web development tasks.

Polls Application
The Polls app is Django's teaching example - a simple voting application. It's used in tutorials because it demonstrates fundamental web concepts without unnecessary complexity: creating questions, offering choices, collecting votes, and displaying results.

Core Components - Definitions & Reasons
1. models.py - Data Definition File
Definition: The file where you define your application's data structure using Python classes that represent database tables.

Reason:

Abstraction: You define data in Python instead of raw SQL
Database Independence: Same code works with SQLite, PostgreSQL, MySQL, etc.
Automatic Features: Gets automatic ID fields, relationships, and query methods
Centralized Structure: All data definitions live in one logical place

2. views.py - Application Logic File
Definition: The file containing functions or classes that process web requests and determine what response to send back.

Reason:

Request Handling: Manages what happens when users visit different pages
Business Logic: Contains the actual application rules and processing
Data Preparation: Gets data from models and prepares it for display
Separation: Keeps logic separate from data (models) and presentation (templates)

3. urls.py - URL Routing File
Definition: The file that maps web addresses (URLs) to specific view functions or classes.

Reason:

Organization: Provides a clean, centralized URL configuration
Readability: Makes it easy to see all available pages in your app
Maintainability: URL changes don't require modifying view logic
Named URLs: Allows referring to URLs by name instead of hardcoding paths

4. admin.py - Administration Configuration File
Definition: The file where you register your models to make them manageable through Django's automatic admin interface.

Reason:

Instant Admin Panel: Get a working admin area without building it yourself
Data Management: Allows non-technical users to manage application data
Customization: Can tailor how models appear and behave in admin
Security: Built-in permission and authentication systems

How These Components Work Together
When someone visits a poll page:

Django checks the urls.py to find which view handles that address
The views.py function receives the request
The view uses models.py to fetch data from the database
The view sends the data to a template for presentation
The final webpage is sent to the user

When an administrator needs to add questions:

They log into the admin site
admin.py determines what models they can access and how they're displayed
Changes are saved through the models to the database
These changes immediately appear on the public site

Why This Structure is Effective

Separation of Concerns: Each component has one job, making code easier to understand and maintain.
Modularity: You can work on one part without breaking others.
Reusability: Models and views can be reused across different parts of the application.
Scalability: This pattern works for small projects and scales to large applications.
Consistency: All Django projects follow similar patterns, making them easier to learn and navigate.

The Polls app demonstrates these concepts in their simplest form, showing how even basic applications benefit from Django's organized approach to web development.