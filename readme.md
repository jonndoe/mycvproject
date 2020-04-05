cvproject demo project
=======================

This is a cvproject based on [Wagtail CMS](https://github.com/wagtail/wagtail).


**Document contents**

- [Installation](#installation)
- [Next steps](#next-steps)
- [Contributing](#contributing)
- [Other notes](#other-notes)

# Installation

- [Docker](#setup-with-docker)
- [Virtualenv](#setup-with-virtualenv)


If you're new to Python and/or Django, we suggest you run this project on a Virtual Machine using Vagrant or Docker (whichever you're most comfortable with). Both Vagrant and Docker will help resolve common software dependency issues. Developers more familiar with
virtualenv and traditional Django app setup instructions should skip to [Setup with virtualenv](#setup-with-virtualenv).



Setup with Docker
-----------------

#### Dependencies
* [Docker](https://docs.docker.com/engine/installation/)

### Installation
Run the following commands:

```bash
git clone https://github.com/wagtail/bakerydemo.git
cd bakerydemo
docker-compose up --build -d
docker-compose run app /venv/bin/python manage.py load_initial_data
docker-compose up
```

The demo site will now be accessible at [http://localhost:8000/](http://localhost:8000/) and the Wagtail admin
interface at [http://localhost:8000/admin/](http://localhost:8000/admin/).

Log into the admin with the credentials ``admin / changeme``.

**Important:** This `docker-compose.yml` is configured for local testing only, and is _not_ intended for production use.

### Debugging
To tail the logs from the Docker containers in realtime, run:

```bash
docker-compose logs -f
```

Setup with Virtualenv
---------------------
You can run the Wagtail demo locally without setting up Vagrant or Docker and simply use Virtualenv, which is the [recommended installation approach](https://docs.djangoproject.com/en/1.10/topics/install/#install-the-django-code) for Django itself.

#### Dependencies
* [Virtualenv](https://virtualenv.pypa.io/en/stable/installation/)
* [VirtualenvWrapper](https://virtualenvwrapper.readthedocs.io/en/latest/install.html) (optional)

### Installation

With [PIP](https://github.com/pypa/pip) and [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/)
installed, run:

    mkvirtualenv wagtailbakerydemo
    cd ~/dev [or your preferred dev directory]
    git clone https://github.com/wagtail/bakerydemo.git
    cd bakerydemo
    pip install -r requirements.txt

Next, we'll set up our local environment variables. We use [django-dotenv](https://github.com/jpadilla/django-dotenv)
to help with this. It reads environment variables located in a file name `.env` in the top level directory of the project. The only variable we need to start is `DJANGO_SETTINGS_MODULE`:

    $ cp bakerydemo/settings/local.py.example bakerydemo/settings/local.py
    $ echo "DJANGO_SETTINGS_MODULE=bakerydemo.settings.local" > .env

To set up your database and load initial data, run the following commands:

    ./manage.py migrate
    ./manage.py load_initial_data
    ./manage.py runserver

Log into the admin with the credentials ``admin / changeme``.

# Next steps

Hopefully after you've experimented with the demo you'll want to create your own site. To do that you'll want to run the `wagtail start` command in your environment of choice. You can find more information in the [getting started Wagtail CMS docs](http://wagtail.readthedocs.io/en/latest/getting_started/index.html).


# Other notes

### Note on demo search

Because we can't (easily) use ElasticSearch for this demo, we use wagtail's native DB search.
However, native DB search can't search specific fields in our models on a generalized `Page` query.
So for demo purposes ONLY, we hard-code the model names we want to search into `search.views`, which is
not ideal. In production, use ElasticSearch and a simplified search query, per
[http://docs.wagtail.io/en/v1.13.1/topics/search/searching.html](http://docs.wagtail.io/en/v1.13.1/topics/search/searching.html).

### Sending email from the contact form

The following setting in `base.py` and `production.py` ensures that live email is not sent by the demo contact form.

`EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'`

In production on your own site, you'll need to change this to:

`EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'`

and configure [SMTP settings](https://docs.djangoproject.com/en/1.10/topics/email/#smtp-backend) appropriate for your email provider.

### Ownership of demo content

All content in the demo is public domain. Textual content in this project is either sourced from Wikipedia or is lorem ipsum. All images are from either Wikimedia Commons or other copyright-free sources.
