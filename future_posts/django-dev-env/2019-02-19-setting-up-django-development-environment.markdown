# Setting Up Your Django Development Environment (For Beginnners):

How to set up a Django development environemnt.

I recently delved into designing sites with Django. Django is described
as "[a high-level Python Web framework that encourages rapid development
and clean, pragmatic design](https://www.djangoproject.com/)". I have
recently been doing some research into the benefits of Django to see if
it might be a good solution for a new web service in development at my
current company.

One thing I struggled with was getting the basic development environment
set up correctly on my machine. Multiple versions of Python are able
to be installed on a single machine and sometimes this can create issues
with knowing exactly where one executable is being called from within the
whole web of Python installments you have on your computer. I fought and
struggled with this for a bit before realizing the answer all along was
a virtual environment.

I wanted to take some time to document what I learned and hopefully someone
can learn from my mistakes. Maybe you are already familiar with
virtual environments and don't need the help at all, but below is an
explanation of exactly the steps I took to create a stable Django development
environment.

Sections:

* [Ensure Python is installed](#ensure-python-is-installed)

## Ensure Python is installed:
- Install latest version of python. (Python3.7.2) using Windowsx86-64 executable installer from [python.org](https://www.python.org/downloads/release/python-372/)
- Check Python version:
```
$ python --version
Python 3.7.2
```
or
```
$ py --version
Python 3.7.2
```

#### Creating a Django Project

- Create a directory for you project and `cd` into it:
```
$ mkdir django_project
$ cd django_project
```

- Create and start a virtual environment (install virtualenv if necessary):
```
$ python -m pip install virtualenv
```
```
$ virtualenv env
$ source env/Scripts/activate
```
This should show `(env)` above command line

- Install Django in virtual environment:
```
$ pip install django
```
Check to see if installed
```
$ py -m django --version
2.1.5
```
Check to see that the current running installation of django is running within
the virtual environment. It should look similar to below:
```
$ where django-admin
C:\...\env\Scripts\django-admin.exe
```
- Start a new project
```
$ django-admin startproject new_django_project
$ ls
env/ new_django_project/
```

- `cd` into project and run the server for the first time.
```
$ cd new_django_project
$ ls
manage.py new_django_project/
```
```
$ py manage.py runserver
```
```
Performing system checks...
System check identified no issues (0 silenced).
You have 15 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, a
uth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.
February 05, 2019 - 16:04:39
Django version 2.1.5, using settings 'new_django_project.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.
[05/Feb/2019 16:04:42] "GET / HTTP/1.1" 200 16348
[05/Feb/2019 16:04:42] "GET /static/admin/css/fonts.css HTTP/1.1" 304 0
[05/Feb/2019 16:04:42] "GET /static/admin/fonts/Roboto-Bold-webfont.woff HTTP/1.1" 304 0
[05/Feb/2019 16:04:42] "GET /static/admin/fonts/Roboto-Regular-webfont.woff HTTP/1.1" 304 0
[05/Feb/2019 16:04:42] "GET /static/admin/fonts/Roboto-Light-webfont.woff HTTP/1.1" 304 0
```

- Open page at `localhost:8000` in your browser. You should be able to see the following page:

![server running](images/django_successfully_installed.png)

- Press `Ctrl + C` to quit the server.
- Run command `deactivate` to exit virtual environment.

#### Open project in Pycharm:
- Open PyCharm and select **File** > **:open_file_folder: Open...**
- Select and open the Django project you have created above:  
![Open django project in PyCharm](images/django_new_project.png)  
You should now see the project imported into PyCharm as such:  
![Imported PyCharm project](images/django_pycharm_project.png)
- Check that the virtual environment is set up appropriately within Pycharm.
Go to **File** > **:wrench: Settings...**. Select **Project:new_django_project** >
**Project Interpreter**. Navigate to the `Scripts` directory under your virtual environment
that was set up above and select `python.exe`. It should look similar to the following:  
![Django Pycharm project virtualenv](images/django_pycharm_virtualenv.png)  
