# DAY 9 WORK:
### LEARNED HOW TO USE FORMS AND TEMPLATES AND MODEL CLASS:
1. Today we have two type of files and classes .
2. Home_html
polls/views.py

from django .shortcuts import render
from .forms import InputForm

def home_view(request):
    context = {}
    context['forms'] = InputForm()
    return render(request, "home.html", context)

polls/urls.py

from .views import home_view, form_view
path('home/', views.home_view, name="home_view"),

3. forms file is created in polls which consist of an inputform class.

class InputForm(forms.Form):
    first_name = forms.CharField(max_length = 200)
    last_name = forms.CharField(max_length = 200)
    roll_number = forms.IntegerField(help_text = "Enter 6 digit roll number")
    password = forms.CharField(widget = forms.PasswordInput())
    
4. created an html file.

<!DOCTYPE html>
<html>

<head>
    <title>Hemanth Survey</title>
</head>

<body>
    <h2>Student Information</h2>
    <form action="" method="post">
        {% csrf_token %}
        {{ forms.as_p }}
        <input type="submit" value="Submit">
    </form>
</body>

#### FINAL OUTPUT:
![alt text](<Screenshot 2026-02-05 095948.png>)

### Created another form that will take user input and store:

polls/template/form_html

<form action="" method="GET">
    <label for="title">title: </label>
    <input id="title" type="text" name="title" required>
    
    <br><br>
    
    <label for="description">Description: </label>
    <input id="description" type="text" name="description" required>    
    <input type="submit" value="Submit">
</form>

polls/models.py

class FormModel(models.Model):
    #fields of the model
    title = models.CharField(max_length = 200)
    description = models.TextField()
    # last_modified = models.DateTimeField(auto_now_add = True)
    # img = models.ImageField(upload_to = "images/")
    #renames the instances of the model
    #with their title names
    
    def __str__(self):
        return self.title

polls/forms.py

#import FormModel form model.py
from .models import FormModel
from django import forms
class Form(forms.ModelForm):
    class Meta:
        model = FormModel
        fields = "__all__"

polls/views.py

from django.shortcuts import render

def form_view(request):
    if request.method == "POST":
        # Get all POST data
        print(request.POST)  # prints the POSTed data as a QueryDict
        title = request.POST.get('title')
        description = request.POST.get('description')
        print(f"title: {title}")
        print(f"description: {description}")
        print(f"Query data: {request.GET}")
        
    return render(request, "form.html")

and finally path:
path("form",views.form_view, name='form_view'),

