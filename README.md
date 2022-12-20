# Django Notes

#### Frameworks
* A framework is a reusable "semi-complete" application that can be specialised to produce custom apps - don't need to worry about the plumbing at the lower level.

#### Model View Controller (MVC)
* A common programming paradigm or pattern.
* A way of working with data in its 3 different manifestations: 
    - at rest (model)
    - in motion (controller)
    - as presented (view)

#### Django
* Django is a full framework i.e. It comes with many preinstalled components and lots of low level automation, but it can be less flexible than a micro framework.
* A project within Django is a container for your site. Its a collection of apps which are self-contained pieces of functionality.
* Django's use of MVC architecture is slightly different as views can also act as controllers.
* Model View Template (MVT) is used, where the view is the controller in the MVC and the template is the user interface.
* It takes Python classes known as models and automatically creates db tables and their relationships.
* Templates are the presentation layer which define how info is displayed to the end user.
* Jinja can also be used as a template language.

#### Getting Set Up
* Download the stable Django 3.2 version, use `pip3 install 'django<4'`.
* The change to the suitable directory based on the Python you are using: `$ cd /workspace/.pip-modules/lib/python3.8/site-packages/`
* To check if successfully downloaded, `ls -la` should provide a list of downloaded libraries, including Django, but go back to the original directory using `cd -`, as changing anything in the site packages could corrupt them or the Python download.
* To start the project, type into the terminal `django-admin startproject project_name .` wher the full stop signifies that the project should be created in the current directory.
* This creates `manage.py`, which is management utility required throughout the project and a project directory of the name provided is created.
* 4 files are created within the project directory:
    - `__init__.py` - to tell our project that this is a directory we can import from.
    - `settings.py` - contains the global settings for our entire Django project.
    - `urls.py` - contains routing info.
    - `wsgi.py` - allows our web server to communicate with our Python application.
* Make sure secret key in `settings.py` is saved in `env.py` file in the form:
\
&nbsp;
`os.environ[SECRET_KEY] = insert secret key`
\
&nbsp;
and in the setting file:
\
&nbsp;
`SECRET_KEY = os.getenv("SECRET_KEY")`

* To run the project: `python3 manage.py runserver`

#### Creating an App:
* Type `python3 manage.py startapp app_name` in the terminal to create an app.
* Note the `urls.py` website can be used to activate functions in `views.py` within the project folder and requires the built-in `path` function, which takes 3 arguments:
    - the url that the user is going to type in (make sure to add this to end of the default url when running the project to see the app)
    - the view function that it's going to return
    - a name parameter

#### Templating:
* When creating a templates folder within the app folder, create another folder within it with the same name as the app. The reason that we're creating this secondary folder inside the templates directory is because when Django looks for templates inside of these apps it will always return the first one that it finds. So by separating it into a folder that matches its app name. We can ensure that we're getting the right template even if there's another template of the same name in a different app.
* After updating the `views.py`and `urls.py` files with suitable templates, add the app name to the `INSTALLED_APPS` list within `settings.py` to make sure it can run.

#### Migrations and Admins:
* Migrations are Django's way of converting Python code into database operations.
* `python3 manage.py makemigrations --dry-run` - Note: the dry run flag is used to run a command to see how the programme would run. You can run the command without the `--dry-run` command. It tells Django to convert our Python code into SQL code.
* `python3 manage.py showmigrations` - This shows migrartions done.
* `python3 manage.py migrate --plan` - Note: the plan flag shows what the command would do. **Run this at the after the initial setup to conduct the required migrations!**
* `python3 manage.py createsuperuser` - used to create a way to log in and look at the tables. Put in a username, email and password.
* To login to view the tables go to the url ending with `/admin` and put in your username and password.

#### Models:
* A model is the single, definitive source of information about your data. It contains the essential fields and behaviors of the data you're storing. Generally, each model maps to a single database table.
* Models are created in the `models.py` file within the specific apps folder.
* Create a class, which will be the name of the table within the database and inherit `models.Model` to ensure that your class can do everything the built-in Django `model` class can do.
* You can define all fields within the class, but note that Django automatically creates an `id` field.
* `Charfield()` refers to a field consisting of characters or a string.
* `Booleanfield()` refers to a field consisting of a boolean value.
* After the class is created, make the migrations and a folder is created called `migrations`, which creates a file called `0001_initial.py` containing code creating the db table by converting code into SQL.
* Don't forget to the final migrate using only `migrate`as mentioned above. This is the step that converts the Python insto SQL.
* At this stage we can programmatically update the table, but we won't see it on the admin page, until you register it in the `admin.py`file. This can be done by importing the class within the models file and registering it with `admin.site.register(class_name)`.
* Note: By default all models that inherit the base model class in the `models.py` file will use its built-in string method to display their class name followed by the word "object" e.g. Item object(2). You can change this by overriding the `__str__` method and returning a suitable name.

#### Creating Data:
* Go to `views.py` as views represent the programming logic that allows users to interact with the database through the templates.
* You can query the table by importing the class from `models.py` and insert `class_name.objects.all()` to query the table within the view.
* Convert this to a dict and then add that dict name to the end of the `render` function.
* The variable name for the query above can be called in the html template and `{{ }}` can be used to call a variable, whereas `{% %}` can be used for logic within - similar to Flask.
* Note: if you have an empty database, you can set an `empty` condition in the html template to deal with the situation.

#### Manually Created Forms in Django:
* Just inside the opening form tag whenever we're posting information in Django, we need to add the CSRF or cross-site request forgery token.
* This token is a randomly generated unique value which will be added to the form as a hidden input field when the form is submitted.
* It works to guarantee that the data that is being posted is actually coming from our the specific app and not from another website.
* To obtain details from a form, use the `POST` method and within `views.py` obtain the `POST` values by their name and put them into the db table using `class_name.objects.create(field1=name1, field2=name2)`.

#### Django Created Forms:
* Django also allows you to create forms directly from the models themselves, which handles all form validation.
* This can be done by creating a `forms.py` file in the project folder and inside it, import both `forms` from Django and the model/class from the `models.py` file.
* Create a class for the form, inheriting from `forms.ModelForm`. In order to tell the form which model it's going to be associated with, we need to provide an inner class called `Meta`. This inner class just gives our form some information about itself, like which fields it should render, how it should display error messages, etc.
* Instead of writing out the whole form in HTML, you can render it out as a template variable. Make sure to set the class name to a variable called `model` as this is a requirement from Django.
* You can just call the variable in the `views.py` file which activates the form template within the sutiable HTML file using the double curly brackets.
* In order to format the form, built-in methods, such as `as_p` can be used.
* To check if a row of data or specific item exists, within the suitable function in `views.py`, use the `get_object_or_404(class_name, id=item_id)` function.
* To prepopulate the form, e.g. if editing a speciifc value, within the `ItemForm` function, use `instance=item`.

#### Deleting an Item:
* To delete an item, create a function, which checks if an item exists and if it does, it can be deleted using `item_name.delete()` within the function.

#### Testing:
* A `tests.py`file is automatically created which imports the `TestCase` class - an extension of the Python library `unittest`.
* Create a class to conduct tests and inherit from the `TestCase` class.
* To run the test file, type into the terminal `python3 manage.py test`.
* Even though the default file for tests is `tests.py`, you can break it into separate files to organise the testing e.g. `test_views.py`.
* All test can be ran with the command above, but you can be more specific e.g. `python3 manage.py test app_name.test_specific_page.TestClassName.test_specific_test_required` which runs for a specific test, but can be run to check all the tests within a class or a specific page if you don't want to run all the tests the whole time.
* Sample tests with some explanations can be found in the specific `test_views.py`, `test_models.py` and `test_forms.py` documents in this repo.
* Note: You can also do CRUD operations within a test file.

#### Coverage:
* It checks how much of the code is actually tested.
* It can be installed with `pip3 install coverage`.
* To run the check and produce a report, type into the terminal `coverage run --source=app_name manage.py test`.
* To view the report, type `coverage report` into the terminal afterwards.
* To view a specific interactive report, type in `coverage html`. This creates a specific folder called `htmlcov`. The interactive report is found in the resultant `index.html` file, which can be opened as a regular static webpage: `python3 -m http.server`.
* Open the `htmlcov/` folder and you can click on the individual files to see why they are not 100% tested.
* Note: 100% coverage doesn't mean 100% of tests pass; just that 100% of the code has been tested.

#### Deployment via Heroku CLI:
* In the console, type in `heroku login -i` and in a separate browser tab, go into the [Applications Section](https://dashboard.heroku.com/account/applications) of your heroku dashboard and generate an authorisation.
* Type in your email for the login in the terminal and paste in the authorisation code as the password.
* Heroku uses an ephemeral file system, which means the `db.sqlie3` file is wiped clean every time Heroku runs updates or we redeploy our app.
* Therefore, we need to install a library called `pyscopg2` using `pip3 install pyscopg2-binary` and we also need to install `guincorn` (also called green unicorn) by using `pip3 install gunicorn`.
* Generate a `requirements.txt` file for Heroku so it knows what to install with the project by typing into the terminal `pip3 freeze --local > requirements.txt`.
* To create a Heroku app in the CLI, type `heroku apps:create unique-app-name-with-hyphens-inbetween -- region eu` and you should be able to see it after typing `heroku apps`.

#### ElephantSQL Integration:
* Go to [elephantsql.com](https://www.elephantsql.com/), login with GitHub and create a new instance.
* Copy the URL once the project instance has been created.
* Create a config var called `DATABASE_URL` and set it to the copied URL. Do not use speech marks in Heroku.
* Within the `env.py` file set `os.environ.setdefault("DATABASE_URL", "to_the_copied_url_value")`.
* Install dj-database-url package version 0.5.0 by using `pip3 install dj_database_url==0.5.0` to format the URL into one that Django can use, subsequently updating the `requirements.txt` file again as mentioned above.
* Import the `dj_database_url` library into `settings.py` and comment out the local `DATABASES` variable, setting it to the elephantSQL value like so `DATABASES = {'default': dj_database_url.parse(os.environ.get('DATABASE_URL'))}`.
* Build the db according to the model structure with `python3 manage.py migrate`, which does not transfer existing data.

#### Pushing code from Git to Heroku:
* Firstly generate a `Procfile` which contains `web: gunicorn django_todo.wsgi:application`, as Heroku throws a H14 error without it and won't deploy. This is going to tell `gunicorn` to run using our projects `wsgi` module, which will allow it to handle HTTP requests like run server does in our local development environment.
* To push the git code, use `git push heroku main`, but it will fail due to the lack of static files, but this error can be overriden with `heroku config:set DISABLE_COLLECTSTATIC=1` and the first push command can be run again.
* To reduce issues, deploy via Heroku itself, but make sure you use the correct API key.
* Note: a `development` variable is set up to ensure that the app can still be updated in the local environmetn without disrupting the deployed version on Heroku.
