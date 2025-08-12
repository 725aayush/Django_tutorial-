# Django Complete Tutorial

## 1. Introduction to Django

Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. Built by experienced developers, it takes care of much of the hassle of web development, allowing you to focus on writing your app without reinventing the wheel.

**Advantages:**
- **Rapid Development** – Build web applications quickly without starting from scratch.
- **Security** – Helps developers avoid common security mistakes such as SQL injection, cross-site scripting (XSS), and cross-site request forgery (CSRF).
- **Scalability** – Suitable for handling high-traffic sites.
- **Versatility** – Can build anything from content management systems to scientific platforms.

---

## 2. Installation

### Prerequisites
- Python 3.8+
- pip (Python package manager)

### Virtual Environment Setup
```bash
# Install virtualenv
pip install virtualenv

# Create virtual environment
virtualenv venv

# Activate (Windows)
venv\Scripts\activate

# Activate (macOS/Linux)
source venv/bin/activate
```

### Install Django
```bash
pip install django
```

Check version:
```bash
django-admin --version
```

---

## 3. Creating a Django Project

To create a new Django project:
```bash
django-admin startproject myproject
cd myproject
```

**Project Structure:**
```
myproject/
    manage.py
    myproject/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

---

## 4. Django Applications

A Django application is a Python package that performs a specific functionality in your project.

**Create an App:**
```bash
python manage.py startapp myapp
```

Add it to `INSTALLED_APPS` in `settings.py`:
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    ...
    'myapp',
]
```

---

## 5. Models

Models define the structure of your database tables.

**Example:**
```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    created_at = models.DateTimeField(auto_now_add=True)
```

**Migrate Database:**
```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 6. Views

Views handle the logic of your web application.

**Function-based View:**
```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Hello, Django!")
```

**Class-based View:**
```python
from django.views import View
from django.http import HttpResponse

class HomeView(View):
    def get(self, request):
        return HttpResponse("Hello from Class-based View")
```

---

## 7. Templates

Templates define the HTML layout and structure.

**Example Template (`home.html`):**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Home</title>
</head>
<body>
    <h1>{{ message }}</h1>
</body>
</html>
```

Render in a view:
```python
from django.shortcuts import render

def home(request):
    return render(request, 'home.html', {'message': 'Welcome to Django'})
```

---

## 8. URLs and Routing

In `urls.py`:
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

---

## 9. Forms

**Example Form:**
```python
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
```

Handle in view:
```python
def contact(request):
    if request.method == 'POST':
        form = ContactForm(request.POST)
        if form.is_valid():
            print(form.cleaned_data)
    else:
        form = ContactForm()
    return render(request, 'contact.html', {'form': form})
```

---

## 10. Admin Interface

Enable admin:
```bash
python manage.py createsuperuser
```

Register model in `admin.py`:
```python
from django.contrib import admin
from .models import Product

admin.site.register(Product)
```

---

## 11. Static and Media Files

In `settings.py`:
```python
STATIC_URL = '/static/'
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

Collect static files:
```bash
python manage.py collectstatic
```

---

## 12. Authentication and Authorization

Use Django's built-in authentication:
```python
from django.contrib.auth import authenticate, login, logout
```

**Example Login View:**
```python
def login_view(request):
    if request.method == 'POST':
        username = request.POST['username']
        password = request.POST['password']
        user = authenticate(request, username=username, password=password)
        if user:
            login(request, user)
```

---

## 13. Testing

Django uses Python’s `unittest` framework.

**Example Test:**
```python
from django.test import TestCase
from .models import Product

class ProductTest(TestCase):
    def test_product_creation(self):
        product = Product.objects.create(name="Test Product", price=10.00)
        self.assertEqual(product.name, "Test Product")
```

Run tests:
```bash
python manage.py test
```

---

## 14. Deployment

For production, use Gunicorn + Nginx or deploy on platforms like Heroku, Render, or AWS.

**Example for Heroku:**
```bash
pip install gunicorn dj-database-url psycopg2
```

Add `Procfile`:
```
web: gunicorn myproject.wsgi
```

---

## 15. Best Practices

- Keep secret keys in environment variables.
- Use `.env` files for configuration.
- Always validate and sanitize user input.
- Regularly update Django for security patches.

---

## 16. Conclusion

You’ve learned how to create a Django project, work with apps, models, views, templates, and deploy your app. Django is a powerful framework, and mastering it will allow you to build scalable and secure applications.

---

**Author:** Django Tutorial  
**License:** MIT
