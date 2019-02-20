---
layout: post
title:  "Intro to Django: Setup Your Development Environment"
date:   2019-02-20 5:00:18 -0600
categories:
  - Django
  - Python
  - PyCharm
---

How to set up a Django development environment.

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
whole web of Python installments you might have on your computer. I fought
and struggled with this for a bit before realizing the answer all along
was a virtual environment.

I wanted to take some time to document what I learned and hopefully someone
can learn from my mistakes. Maybe you are already familiar with
virtual environments and don't need the help at all, but below is an
explanation of exactly the steps I took to create a stable Django development
environment.

Sections:

* [Ensure Python is installed](#ensure-python-is-installed)
* [Creating a Django Project](#creating-a-django-project)
* [Open Project in PyCharm](#open-project-in-pycharm)

## Ensure Python is installed:
Django is a Python framework and therefore you need to ensure you have
Python installed on your machine. I decided to go ahead and make sure
I had the most up to date version of Python which at the time of writing
this is 3.7.2.

#### Install latest version of python. (Python3.7.2):

##### Windows:
Using Windowsx86-64 executable installer from
[python.org](https://www.python.org/downloads/release/python-372/)

##### Linux:
On a Linux machine you can run the following command
```
sudo apt install python3.7
```

##### Mac:
I do not have a mac so this is always a bit of a mystery for me but here
is a page to download
[python.org](https://www.python.org/downloads/release/python-372/)


#### Check Python version:
Below are two ways to check what version of Python you have installed

```
$ python --version
Python 3.7.2
```
or
```
$ py --version
Python 3.7.2
```

## Creating a Django Project:

The following walks you through setting up your Django development
environment using a virtual environment to ensure you are running a
Python environment within that virtual environment. This encapsulates
any work and programs installed within the virtual environment. This is
best for not confusing different versions of installed programs.

I've found this to be the best way to run Django and ensure that it is
found by the environment.

1. Create a directory for you project and `cd` into it:
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

- Open page at `localhost:8000` in your browser. You should be able to see the following page:

![server running]({{ '/images/django_successfully_installed.png' | absolute_url }})

- Press `Ctrl + C` to quit the server.
- Run command `deactivate` to exit virtual environment.

## Open project in Pycharm:
If you would like to open this Django project in PyCharm you can follow
the steps below:  
1. Open PyCharm and select **File** > **:open_file_folder: Open...**
- Select and open the Django project you have created above:    
![Open django project in PyCharm]({{ '/images/django_new_project.png' | absolute_url }})  
You should now see the project imported into PyCharm as such:  
![Imported PyCharm project]({{ '/images/django_pycharm_project.png' | absolute_url }})  
- Check that the virtual environment is set up appropriately within Pycharm.
Go to **File** > **:wrench: Settings...**. Select **Project:new_django_project** >
**Project Interpreter**. Navigate to the `Scripts` directory under your virtual environment
that was set up above and select `python.exe`. It should look similar to the following:   
![Django Pycharm project virtualenv]({{ '/images/django_pycharm_virtualenv.png' | absolute_url }})  
