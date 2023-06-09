# The Task ME - API
The TaskME is a Task platform for the modern world. The project manager can create Projects on this platform and assign them to different user.You can view all the tasks and projects and see who dey are assigned to from the Project managers End. In this app you can also see deadlines of the Current Projects ,thier start date of the projects and it end date.Each User is restricted to see the Tasks assigned to them and get more details about each project.This section of the project is the backend API database built to support the ReactJS frontend, and it is powered by the Django Rest Framework.

#### DEPLOYED BACKEND API RENDER [LINK](http://tobi.pythonanywhere.com/)
#### DEPLOYED FRONTEND RENDER [LIVE SITE](https://taskme-9502ad.netlify.app)
#### DEPLOYED FRONTEND [REPOSITORY](https://github.com/diddyjax19/Taskit-FrontEnd)

## Table of Contents
+ [User Stories](#user-stories "User Stories")
+ [Database](#database "Database")
+ [Testing](#testing "Testing")
  + [Validator Testing](#validator-testing "Validator Testing")
  + [Unfixed Bugs](#unfixed-bugs "Unfixed Bugs")
+ [Technologies Used](#technologies-used "Technologies Used")
  + [Main Languages Used](#main-languages-used "Main Languages Used")
  + [Frameworks, Libraries & Programs Used](#frameworks-libraries-programs-used "Frameworks, Libraries & Programs Used")
+ [Deployment](#deployment "Deployment")
+ [Credits](#credits "Credits")
  + [Content](#content "Content")
  + [Media](#media "Media")

## User Stories:
All User Stories have been documented in their own file, the link for which can be found [HERE](static/userstories.md).

I have included links to the [GitHub Issues](https://github.com/CluelessBiker/project5-red-crayon/issues) for this project, as well as the [KANBAN board](https://github.com/users/CluelessBiker/projects/2).

## Database:
![SQL Database model](screenshots/image_2023-04-28_13-03-30.png)

## Testing:
### Validator Testing: 
All files passed through [PEP8](http://pep8online.com/) without error.

![PEP8](/static/images-readme/readme-pep8.png)

### Manual Testing:
1. Manually verified each url path created works & opens without error.
2. Verified that the CRUD functionality is available in each app via the development version: Create Project, Create Task, Create USers, Assign Tasks, Assign Date and time.
 - Checked this by going to each link.
 - Creating a new user.
 - User Login
 - Creating Project with all details.
 - Creating Tasks with all details.
 - Assiging Tasks to the appropraite User.
 - Creating Project Date and time
 - Creating Project Status.
2. - Made sure that each user are not able to see the projects thhatthey are not assigned or are not working on .
    `- Ran migrations file.
 - deleted `0001_initial.py` files & `__pycache__` from the migration folders in all apps.
 - Ran the migration commands again:
```
python3 manage.py makemigrations
python3 manage.py migrate
```
3. - created a new super user to test functionality
```
python3 manage.py createsuperuser
```
 - upon returning to the development version of the app, we were now unable to login or create a new user
 - clearing the browser cookies & cache, as well as relaunching the gitpod workspace resolved this.




## Deployment:
### Project creation:
1. Create the GitHub repository.
2. Create the project app on [PythonAnywhere](pythonanywhere.com).
3. Add the Postgres package to the Heroku app via the Resources tab.
4. Once the GitHub repository was launched on GitPod, installed the following packages using the `pip install` command:
```
'django<4'
Pillow
djangorestframework
django-filter
gunicorn
django-cors-headers
pytz==2023.3
asgiref
Sqlparse
Tzdata
```
5. Add the following to INSTALLED_APPS to support the newly installed packages:
```
 'rest_framework',
  'rest_framework.authtoken'
  'home',
  'api',
  'api.users',
  'api.tasks',
  'api.projects'
'corsheaders',
```

6. Import the database, the regular expression module & the env.py
```
import dj_database_url
import re
import os
if os.path.exists('env.py')
    import env
```
7. Added a media Root to the settings.py
```
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR,'media')
```
8. Added a Static path to the settings.py

# [Configured static file](https://docs.djangoproject.com/en/3.2/howto/static-files/)
```
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```
9. Below INSTALLED_APPS, set site ID:

SITE_ID = 1


10. Below BASE_DIR, create the REST_FRAMEWORK, and include page pagination to improve app loading times, pagination count, and date/time format:

                    REST_FRAMEWORK = {
                        'DEFAULT_PERMISSION_CLASSES': [
                            'rest_framework.permissions.IsAuthenticated',
                        ],
                        'DEFAULT_AUTHENTICATION_CLASSES': [
                            'rest_framework.authentication.TokenAuthentication',
                        ],
                        'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.LimitOffsetPagination',
                        'PAGE_SIZE': 10

                    }

11. Then added this to the searializers.py

                    REST_AUTH_SERIALIZERS = {
                        'USER_DETAILS_SERIALIZER': 'project_name.serializers.CurrentUserSerializer'
                    }

12. Updated DEBUG variable to:

            DEBUG = 'False'

13. Updated the DATABASES variable to:

                DATABASES = {
                    'default': ({
                    'ENGINE': 'django.db.backends.sqlite3',
                        'NAME': BASE_DIR / 'db.sqlite3',
                    } if 'DEV' in os.environ else dj_database_url.parse(
                        os.environ.get('DATABASE_URL')
                    )
                    )
                }

14. Added the pythonanywhere app link to the ALLOWED_HOSTS variable:

ALLOWED_HOST = ['tobi.pythonanywhere.com']


15. Below ALLOWED_HOST, added the CORS_ALLOWED variable as shown in [DRF-API walkthrough](https://learn.codeinstitute.net/courses/course-v1:CodeInstitute+DRF+2021_T1/courseware/a6250c9e9b284dbf99e53ac8e8b68d3e/0c9a4768eea44c38b06d6474ad21cf75/?child=first):
```
if 'CLIENT_ORIGIN' in os.environ:
    CORS_ALLOWED_ORIGINS = [
        os.environ.get('CLIENT_ORIGIN')
    ]

if 'CLIENT_ORIGIN_DEV' in os.environ:
    extracted_url = re.match(r'^.+-', os.environ.get('CLIENT_ORIGIN_DEV', ''), re.IGNORECASE).group(0)
    CORS_ALLOWED_ORIGIN_REGEXES = [
        rf"{extracted_url}(eu|us)\d+\w\.gitpod\.io$",
    ]
```
16. Also added to the top of MIDDLEWARE:

'corsheaders.middleware.CorsMiddleware',

- During a deployment issue, it was suggested by a fellow student, Johan, to add the following lines of code below CORS_ALLOW_CREDENTIALS:


CORS_ALLOW_ALL_ORIGINS = True
CORS_ALLOW_HEADERS = list(default_headers)
CORS_ALLOW_METHODS = list(default_methods)
CSRF_TRUSTED_ORIGINS = [os.environ.get(
    'CLIENT_ORIGIN_DEV', 'CLIENT_ORIGIN',
)]


17. In addition, Johan also suggested to add the following import statement at the top of the settings.py file:

from corsheaders.defaults import default_headers, default_methods

### Final requirements:

18. Migrated the database:
```
python3 manage.py makemigrations
python3 manage.py migrate
```
19. Froze requirements:
```
pip3 freeze --local > requirements.txt
```
20. Added, committed & pushed the changes to GitHub
21. Navigated back to heroku, and under the ‘Deploy’ tab, connect the GitHub repository.
22. Deployed the branch.

### Manual Deplyoment In Python Anywhere:

1. - Uploading your code to PythonAnywhere
    Assuming your code is already on a code sharing site like GitHub or Bitbucket, you can just clone it from a Bash Console:

![screenshot 1](screenshots/1.PNG)

2. Create a virtualenv and install Django and any other requirements
    In your Bash console, create a virtualenv, naming it after your project, and choosing the version of Python you want to use:

![screenshot 2](screenshots/2.PNG)
  Warning: Django may take a long time to install. PythonAnywhere has very fast internet, but the filesystem access can be slow, 
  and Django creates a lot of small files during its installation. Thankfully you only have to do it once!

TIP: if you see an error saying mkvirtualenv: command not found, check out InstallingVirtualenvWrapper.

3. Setting up your Web app and WSGI file
    At this point, you need to be armed with 3 pieces of information:

  The path to your Django project's top folder -- the folder that contains "manage.py", eg /home/myusername/mysite
  The name of your project (that's the name of the folder that contains your settings.py), eg mysite
  The name of your virtualenv, eg mysite-virtualenv
  Create a Web app with Manual Config
  Head over to the Web tab and create a new web app, choosing the "Manual Configuration" option and the right version of Python (the same one you used to create your virtualenv).

![screenshot 3](screenshots/3.PNG)

4. NOTE: Make sure you choose Manual Configuration, not the "Django" option, that's for new projects only.
    Enter your virtualenv name
    Once that's done, enter the name of your virtualenv in the Virtualenv section on the web tab and click OK.

![screenshot 3.1](screenshots/3.1.PNG)

5. You can just use its short name "mysite-virtualenv", and it will automatically complete to its full path in /home/username/.virtualenvs.

    Optional: enter path to your code
    Although this isn't necessary for the app to work, you can optionally set your working directory and give yourself a convenient hyperlink to your source files from the web tab.

    Enter the path to your project folder in the Code section on the web tab, eg /home/myusername/mysite in Source code and Working directory

![screenshot 3.2](screenshots/3.2.PNG)

6. Edit your WSGI file
    One thing that's important here: your Django project (if you're using a recent version of Django) will have a file inside it called wsgi.py. This is not the one you need to change to set things up on PythonAnywhere -- the system here ignores that file.

    Instead, the WSGI file to change is the one that has a link inside the "Code" section of the Web tab -- it will have a name something like /var/www/yourusername_pythonanywhere_com_wsgi.py or /var/www/www_yourdomain_com_wsgi.py.

    Click on the WSGI file link, and it will take you to an editor where you can change it.

    Delete everything except the Django section and then uncomment that section. Your WSGI file should look something like this:

![screenshot 4](screenshots/4.PNG)

7. Be sure to substitute the correct path to your project, the folder that contains manage.py, which you noted above.
    Don't forget to substitute in your own username too!
    * Also make sure you put the correct value for DJANGO_SETTINGS_MODULE.
    * This guide assumes you're using a recent version of Django, so leave the old wsgi.WSGIHandler() code commented out, or better still, delete it.
  
8. Save the file, then go and hit the Reload button for your domain. (You'll find one at the top right of the wsgi file editor, or you can go back to the main web tab).

9. Database setup
    If, like most sites, your site uses a database, you'll need to set that up. Go to the Consoles tab, start a bash console, use cd to navigate to the directory where your Django project's manage.py lives, then run.

![screenshot 5](screenshots/5.PNG)


## Technologies Used:
### Main Languages Used:
- Python

### Frameworks, Libraries & Programs Used:
- Django
- Asgiref
- Django-cors-headers
- Pytz
- Sqlparse
- Django RestFramework
- Pillow
- Django Rest Auth
- PostgreSQL
- Cors Headers
- DrawSQL: An interactive ERD platform that allows you to set up your database tables, & build the connections between them for a visual layout.


## CREDITS:

### Content:
- The creation of this API database was provided through the step by step guide of the C.I. DRF-API walkthrough project.
- All classes & functions have been credited.
- Modifications have been made to the 'Profiles' & 'Posts' app models, and an additional two apps along with models, serializers & views have been created by me.
- Oisin from Tutor support went above & beyond to assist me in resolving an issue with my database that prevented new posts from being created. The steps we took have been documented in point #5 of the Manual Testing section.

### Media:
- Default post image Photo by Artem Podrez from [Pexels](https://www.pexels.com/photo/image-of-a-whale-made-of-scrap-materials-7048043/)
- Default profile image from [Favicon](https://favicon.io/emoji-favicons/alien-monster)
