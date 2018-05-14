# Exercise 7

## Introduction
This is a how-to. Follow the instructions and take thorough notes.

## Background
HTML provides form tags allowing you to define name/value pairs to be submitted to another script. However, HTML alone does not allow one to capture those name/value pairs again in the receiving page. You must have a web script on the target web server to receive and process them.

Web scripts are essentially programs giving dynamic capabilities to web pages in response to GET, POST, PUT, and DELETE requests. You may recognize GET and POST as they are two different submission methods of an HTML form element.

Web scripts may be written in a Compiled Language where source code is first converted into a machine specific set of instructions and then saved as an executable file, before being run by the host computer as needed.

They may also be written in an Interpreted Language such as Python, where source code is converted to a machine specific set of instructions at runtime, when a user submits a request.

Web Scripts have two purposes:
1. To help build graphical user interfaces like UTDirect.
2. To provide an interface through which client applications may query, retrieve, update, and perform processes via JSON, XML, and other document formats.

During this exercise you will learn how to write, save, and execute Python/Django scripts to render pages.

Most of our scripts run on centrally maintained servers (we deploy our code to them) but we will be using a virtual machine as a software based emulation of a computer. PyPE tools use a Vagrant Instance running on a VirtualBox virtualization platform for local development and testing.

## Setup
1. Setup your server - ask your trainer to help with this.
2. [Configure your virtual machine](https://wikis.utexas.edu/display/python/Pype+Developer+VM+Setup)
    * A virtual machine is a software based emulation of a computer. The current and subsequent releases of PyPE tools will use a Vagrant Instance running on a VirtualBox virtualization platform. On the PyPE Developer VM page, follow the Initial setup instructions to configure your virtual machine.
> **Note**
> When you follow the link to setup your VM, the page you land on has a link on the right, "VM setup for trainees." Just use the info on the page you land on and ignore the other page because the information on there is out of date.
3. Setup your IDE

## Your First Web Script
### Create Your Project

If you have a special challenge library, then you will have a group that matches that name. If you are using your personal library, then your group should be user_<eid>.

1. In [PyPE](https://pype.its.utexas.edu/), click on the 'Create Project' link and create a project called djangotraining within your project group.
2. Under Project Details, enter "Django Training" in the Name field and "djangotraining" in the Abbr field.
3. Use the most current version of the PyPE tools
4. Leave the Description field blank.

You may now view your project in GitHub. All projects within PyPE start with utdirect. The <group> portion of the name is referred to as the "project group."

### Setup your local project directory
On your machine, set up a project directory. To do this, follow the directions for [Working with a new project group](https://wikis.utexas.edu/pages/viewpage.action?title=Pype+Developer+VM+Setup&spaceKey=python#PypeDeveloperVMSetup-Workingwithanewprojectgroup).

### Check Out Your Project
Now you will need to check out your project from the repository. If you are unsure how to do this, ask your trainer for instructions.

### Create An Application
Now that you have a project, create an application within the project.

1. Back in the shell, change directories to your project.
    * cd /pype/<group>/djangotraining
2. Run the following command:
    * python manage.py startapp web_dev
    * Alternatively you can write:  ./manage.py startapp web_dev
    * The "./" serves as a shortcut of sorts.

### Edit settings.py
1. In your editor, open the settings.py file in your project.
2. Close to the end of the file, change the commented out line within the list of INSTALLED_APPS. so that it reads
    * '<group>.djangotraining.web_dev',

### Edit urls.py
> Although you will delve deeper into the Django URL Dispatcher in the future, the importance of well designed urls.py files can not be stressed enough.
> While this is a small project with one application, it is a good rule of thumb to have multiple urls.py for maintainability and extensibility.

1. Modify the urls.py file in the djangotraining directory so that it references the urls.py of your application.
```python
    from django.conf.urls import include, url


    urlpatterns = [
        url(r'^apps/dpch_/djangotraining/web_dev/', include('dpch_.djangotraining.web_dev.urls', namespace='web_dev'))
    ]
```
2. Similarly you will need to add a urls.py file within your application to reference the correct view.
```python
    from django.conf.urls import url

    from <group>.djangotraining.web_dev.views import index

    urlpatterns = [
        url(r'^$', index, name='index')
    ]
```
### Edit views.py
1. In the views.py file which is in the web_dev directory, create a function called index. All it will do is accept a parameter called request and return this:
```html
HttpResponse("<h2>My First Web Dev page!</h2>")
```
2. In order for this to work, you'll need to add the following line at the top of your views.py file:
    * from django.http import HttpResponse

### Test your initial application build
The first few steps of this part depend on how you're running things. Ask your trainer if you need help getting started.

1. Type a version of the following:
    * pype_workon <pype_tools> -p <group>.<project>
    *     e.g. pype_workon 27.11.0 -p user_lhl279.djangotraining
    * For more information on pype_workon, read this documentation.
2. Start your local server
    * python manage.py runsslserver
    * Or you can use the shortcut like earlier: ./manage.py runsslserver
3. Open a browser and go to:
    * https://local.utexas.edu:8000/apps/\<group>/djangotraining/web_dev/
4. This address targets the local Django development server that is running on your own machine, so that the responses returned are generated by the code that's running on your computer. Your computer should have been set up to point the server name "local.utexas.edu" to the IP address 127.0.0.1, which is the address reserved to point to a local machine. If you have any trouble at this step, see the "Hosts File" section of this page in the PyPE documentation.

### Rendering Templates for your Application
The functions in the views.py file will eventually be quite complicated so having to pass all of the html for the page to the HttpResponse object would prove harrowing and cumbersome. Fortunately Django makes rendering html easy by letting us pass entire files to a special function called render.
1. Create a new directory named templates within your app directory. Within the templates directory, create another director with the same name as your app so the path will look like this: <group>/djangotraining/web_dev/templates/web_dev(the reasons for this will be explained later). Make a copy of your hangman project and save it within the web_dev/templates/web_dev directory. Keep the original as it was. _MATT - WHAT ELSE DO WE NEED TO MAKE THIS WORK?_
2. Change your views.py file so that it looks like this:
```python
    from django.shortcuts import render

    def index(request):
        return render(request=request, template_name='web_dev/Web1BusinessProblem.html')
```
substituting the name of your .html file for the one shown above.
3. Reload your Web page to see your form.

**Congratulations!** You've just put up your first Web pages using Python (and Django).
