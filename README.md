# Django Blog

## Starting a new project
1. Within the newly generated repository, click on projects on the bar at the top
2. Click on 'New Project', give it a sensible name and select 'Basic Kanban' from templates
3. Remove the pre-generated notes from the left had side of the new project board
4. Check there are 3 collums (To do, In progress and Complete - or any other sensible names)

To move "new issues" into  the to-do column when they're newly added:
1. Click on the three dots in the top right corner of the to-do column
2. Click on 'manage automation' from the drop down list, click on the 'newly added' checkbox
3. Click on 'Update automation'.

Setting up a User Story Template on GitHub:
1. In the settings tab at the top of the page, scroll down to 'Features' and click on 'Set up template'
2. Click on the drop down list and select 'custom template', then click 'preview and edit'
3. Template name: User Story, About: This is our default user story template, Template Content: As a **role** I can **capability** so that **received benefit**, Issue default title: USER STORY: <title>
4. Click on the 'X' in the top right corner of this field, and then click on 'Propose Changes'
5. Write a suitable commit message such as 'Add a new user story temple' and commit the changes

To use the new template:
1. Click on 'Issues' at the top of the page and select 'New Issue', then click 'get started' on the template you just created
2. Add user stories to the template:
    <ol>
        <li>View post list: As a Site User I can view a list of posts so that I can select one to read</li>
        <li>Open a post: As a Site User I can click on a post so that I can read the full text</li>
        <li>View likes: As a Site User / Admin I can view the number of likes on each post so that I can see which is the most popular or viral</li>
        <li>View comments: As a Site User / Admin I can view comments on an individual post so that I can read the conversation</li>
        <li>Account registration: As a Site User I can register an account so that I can comment and like</li>
        <li>Comment on a post: As a Site User I can leave comments on a post so that I can be involved in the conversation</li>
        <li>Like / Unlike: As a Site User I can like or unlike a post so that I can interact with the content</li>
        <li>Manage posts: As a Site Admin I can create, read, update and delete posts so that I can manage my blog content</li>
        <li>Create drafts: As a Site Admin I can create draft posts so that I can finish writing the content later</li>
        <li>Approve comments: As a Site Admin I can approve or disapprove comments so that I can filter out objectionable comments</li>
    </ol>
3. When the title and main body have been filled, click on 'Projects' on the right hand side and select the prjoct to link the issue to.

## New Project Checklist
1. Install Django and the supporting libraries
2. Create a new blank django project and app
3. Set our project to use PostgreSQL and Clouinary
4. Deploy our new empty projct to Heroku

### Install Django and the supporting libraries
> pip3 install django gunicorn<br>
> pip3 install dj_database_url psycopg2<br>
> pip3 install dj3-cloudinary-storage<br>
> pip3 freeze --local > requirements.txt<br>

### Create a new blank django project and app
> django-admin startproject {project_name} .<br>
> python3 manage.py startapp blog

Now we need to add the newly created 'blog' app to the list of installed apps in settings.py
1. Navigate to settings.py
2. At the end of the INSTALLED_APPS list, add "'blog',"
3. Make migrations:
> python3 manage.py migrate


### Set up project to use PostgreSQL and Clouinary

<strong>NOTE: If you get the error below during the steps to deployment:</strong>
django.db.utils.OperationalError: FATAL: role "somerandomletters" does not exist<br>
Run the following command in the terminal to fix it: "unset PGHOSTADDR"

<strong>PostgreSQL</strong>

1. Create a new app on Heroku
2. In the resources tab, search for Postgres in the add-ons box
3. Click on the settings tab and reveal config vars, copy the value of DATABASE_URL
4. At the same level as requirements.txt create a file called 'env.py'
5. Within the env.py, import os, and set up the DATABASE_URL and SECRET_KEY variables to match those in Heroku (generate a secret key from https://miniwebtool.com/django-secret-key-generator/)
6. In settings.py, import os, import dj_database_url, and set an if statement to retrieve the env.py file 
7. Remove the string in the SECRET_KEY variable and replace with os.environ.get('SECRET_KEY')
8. Comment out the DATABASES section in settings.py
9. Make mirgrations: python3 manage.py migrate

<strong>Cloudinary</strong>

1. Log in/Sign up to cloudinary and navigate to the dashboard
2. Copy the API Environment Variable key to the clipbaord
3. Make a new environ in env.py and copy in the URL (removing "CLOUDINARY_URL =" from the beginning)
4. Paste the URL into the Heroku config vars as well
5. In the Heroku config vars add: DISABLE_COLLECTSTATIC and set the value to '1'
6. In the INSTALLED_APPS section above the static files string, add "cloudinary_storage" and above blog, add "cloudinary"
7. Towards the bottom of settings.py in the static files section, add the following:
    <ol>
        <li>STATICFILES_STORAGE = 'cloudinary_storage.storage.StaticHashedCloudinaryStorage'</li>
        <li>STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]</li>
        <li>STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')</li>
        <li>MEDIA_URL = '/media/'</li>
        <li>DEFAULT_FILE_STORAGE = 'cloudinary_storage.storage.MediaCloudinaryStorage'</li>
    </ol>
8. In the buildpaths section of settings.py add: TEMPLATES_DIR = os.path.join(BASE_DIR, 'templates')
9. In the templates section of settings.py for the DIRS key, add the following value: [TEMPLATES_DIR]
10. Add the heroku app into the ALLOWED_HOSTS section: {app_name}.herokuapp.com and add localhost so we can run it locally
11. Create the following directories/folders in the top level (same as requirements.txt): media, static, templates
12. Add a Procfile in the top level of the repository and add the following: web: gunicorn {project_name}.wsgi
13. Deploy via heroku dashboard or via CLI

## Creating our database entity-relationship-diagrams

<img src="readmeimages/posts.png"><br>
<img src="readmeimages/comment.png">

## Creating our database models

1. In models.py, from django.contrib.auth.models import User, from cloudinary.models import CloudinaryField
2. Create the classes needed to fulful all criteria of the ERDs (see models.py for code)
3. python3 manage.py makemigrations
4. python3 manage.py migrate


## Building the admin site

> python3 manage.py createsuperuser
> python3 manage.py runserver

At the end of the URL in the web-browser type /admin and log in using the credentials created

1. Open the admin.py file and at the top, from .models import Post
2. Underneath the imports: @admin.register(Post)

> pip3 install django-summernote
UPDATE THE REQUIREMENTS.TXT FILE: pip3 freeze --local > requirements.txt

Update the INSTALLED_APPS in settings.py with 'django_summernote'

In urls.py update the django.url import with ', include'
In admin.py, from django_summernote.admin import SummernoteModelAdmin

See admin.py for code
Add class for comment control in admin panel

In the project panel on GitHub, move manage posts, approve comments and create drafts into the 'done collumn'.