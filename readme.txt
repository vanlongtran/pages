Ch 3 Lesson Notes
Django for Beginners by William S. Vincent

-Use templates so that individual HTML files can be served by a view to a web page specified by the URL.
-Templates, Views, URLs. This pattern will hold true for every Django web page
you make.
-By default, Django looks within each app for templates. Modify settings.py file to tell Django to also look in this project-level folder for templates.
-We created a single, project-level templates directory that is available to all apps.
-Create a templates/home.html file and templates/about.html 
-Update settings.py
# pages_project/settings.py
TEMPLATES = [
{
...
'DIRS': [os.path.join(BASE_DIR, 'templates')],
...
},
]

In our view we’ll use the built-in TemplateView to display our template. Update the
pages/views.py file.
Chapter 3: Pages app 45
Code
# pages/views.py
from django.views.generic import TemplateView
class HomePageView(TemplateView):
	template_name = 'home.html'
class AboutPageView(TemplateView):
	template_name = 'about.html'

Update the project-level urls.py file. We add include on the second line to
point the existing URL to the pages app. Next create an app-level urls.py file.
When using Class-Based Views, you always add as_view() at the end of the view name.

# pages_project/urls.py
from django.contrib import admin
from django.urls import path, include
urlpatterns = [
path('admin/', admin.site.urls),
path('', include('pages.urls')),
]

Create  pages/urls.py
# pages/urls.py
from django.urls import path
from . import views
urlpatterns = [
	path('', views.HomePageView.as_view(), name='home'),
	path('about/', views.AboutPageView.as_view(), name='about'),
]


Extending Templates and adding header code inherited by all other templates.
-Create templates/base.html
-Template tags take the form of {% something %} where the “something” is the
template tag itself. To add URL links in our project we can use the built-in url template tag which takes the URL pattern name as an argument.

The URL route for our homepage is called home therefore to configure a link to it we
would use the following: {% url 'home' %} .
Code
<!-- templates/base.html -->
<header>
<a href="{% url 'home' %}">Home</a> | <a href="{% url 'about' %}">About</a>
</header>
{% block content %}
{% endblock %}

At the bottom we’ve added a block tag called content . Blocks can be overwritten by
child templates via inheritance.

 update our home.html and about.html to extend the base.html template.
 <!-- templates/home.html -->
{% extends 'base.html' %}
{% block content %}
<h1>Homepage.</h1>
{% endblock %}

<!-- templates/about.html -->
{% extends 'base.html' %}
{% block content %}
<h1>About page.</h1>
{% endblock %}

Run server.

Automated tests let us write one time how we expect a specific piece of our project to behave and then let the computer do the checking for us.
Write test code in tests.py file
 $ python manage.py test