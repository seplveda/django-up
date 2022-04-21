# django-up

`django-up` is a zero configuration [Django][django] deployment tool to deploy a project to a Debian 11 environment.


```shell
> ./manage.py up djangoup.com
```

Running `django-up` will deploy a production ready, SSL-enabled, Django application to a VPS using:

- Nginx
- Gunicorn
- PostreSQL
- SSL with acme.sh
- UFW
- OpenSMTPd


## Setup

Ensure that `ansible` is installed globally.

Add `django-up` as a git submodule:

```shell
> git submodule add git@github.com:sesh/django-up.git up
```

Install `pyyaml` and `dj_database_url` as dependencies in your project.

Add `up` install your `INSTALLED_APPS` to enable the management command:

```python
INSTALLED_APPS = [
  'up',
  # ...
]
```

Add your target domain to the `ALLOWED_HOSTS` in your `settings.py`.

Set the `SECURE_PROXY_SSL_HEADER` setting in your `settings.py` to ensure the connection is considered secure.

```python
SECURE_PROXY_SSL_HEADER = ('HTTP_X_SCHEME', 'https')
```

Configure Django to load the SECRET_KEY from your environment, and add a secure secret key to your `.env` file:

```python
SECRET_KEY = os.environ["DJANGO_SECRET_KEY"]
```

`.env`:

```
DJANGO_SECRET_KEY=dt(t9)7+&cm$nrq=p(pg--i)#+93dffwt!r05k-isd^8y1y0
```

Set up your database to use `dj_database_url`:

```python
import dj_database_url
DATABASES = {
    'default': dj_database_url.config(default=f'sqlite:///{BASE_DIR / "db.sqlite3"}')
}
```

Make sure that `GUNICORN_PORT` is set to a unique port for the server that you are deploying to:

```python
GUNICORN_PORT = 8556
```

Deploy with the `up` management command:

```shell
> ./manage.py up yourdomain.example
```


### Setting environment variables

Add environment variables to a `.env` file alongside your `./manage.py`. These will be exported into the environment before running your server (and management commands).


  [django]: https://www.djangoproject.com
