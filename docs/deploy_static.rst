Collecting Static Files
============================================

.. contents::
    :depth: 2

In the Django documentation, **static files** are files linked into your html documents like ``.css`` and ``.js`` as well as images files like ``.png`` and ``.jpg``. These are served directly by your web server (Apache, Nginx, etc.) rather than by Django because they don't require any processing. Their contents are just copied to the browser. Serving static files is what web servers were written for.

Django-Mako-Plus works with static files the same basic way that traditional Django does -- with one difference: the folder structure is different in DMP.  DMP-enabled apps contain the following directories:

::

    app/
        media/
        scripts/
        styles/
        templates/
        views/

At deployment, collect static files out of these directories with the following command:

::

    python3 manage.py dmp collectstatic

If your project contains both DMP and regular Django apps, you can collect static files with both commands:

::

    python3 manage.py collectstatic
    python3 manage.py dmp collectstatic --overwrite

Setup
---------------------------

In your project's settings.py file, you should have the following.  These settings are not specific to DMP, and they are described in the `Django documentation <https://docs.djangoproject.com/en/dev/howto/static-files/>`_.

.. code:: python

    STATIC_URL = '/static/'
    STATICFILES_DIRS = (
        BASE_DIR,
    )
    STATIC_ROOT = os.path.join(BASE_DIR, 'static')

**Note that the last line is a serious security issue if you go to production with it** (although Django disables it as long as ``DEBUG=False``). More on that later.

Also in settings.py, ensure you have Django's static files app enabled:

.. code:: python

    INSTALLED_APPS = [
        ...
        'django.contrib.staticfiles',
        ...
    ]


Linking to Static Files
---------------------------

During development, place media files for the homepage app in the homepage/media/ folder. This includes images, videos, PDF files, etc. -- any static files that aren't Javascript or CSS files.

Reference static files using the ``${ STATIC_URL }`` variable. For example, reference images in your html pages like this:

.. code:: html

    <img src="${ STATIC_URL }homepage/media/image.png" />

By using the ``STATIC_URL`` variable from settings in your urls rather than hard-coding the ``/static/`` directory location, you can change the url to your static files easily in the future.

Deployment
---------------------------

At production/deployment, comment out ``BASE_DIR`` because it essentially makes your entire project available via your static url (a serious security concern):

.. code:: python

    STATIC_URL = '/static/'
    STATICFILES_DIRS = (
        # BASE_DIR,
    )
    STATIC_ROOT = os.path.join(BASE_DIR, 'static')

When you deploy to a web server, run ``dmp collectstatic`` to collect your static files into a separate directory (called ``/static/`` in the settings above):

::

    python3 manage.py collectstatic
    python3 manage.py dmp collectstatic --overwrite

Point your web server (Apache, Nginx, IIS, etc.) to serve this folder directly to browsers. For example, in Nginx, you'd set the following:

::

    location /static/ {
        alias /path/to/your/project/static/;
        access_log off;
        expires 30d;
    }

In Apache, you'd set the following:

::

    Alias /static/ /path/to/your/project/static/
    <Directory /path/to/your/project/static/>
        Order deny,allow
        Allow from all
    </Directory>

Advanced Use
---------------------------

``dmp collectstatic`` will refuse to overwrite an existing ``/static/`` directory. If you already have this directory (either from an earlier run or for another purpose), you can 1) delete it before collecting static files, or 2) specify the overwrite option as follows:

::

    python3 manage.py dmp collectstatic --overwrite

If you need to ignore certain directories or filenames, specify them with the ``--skip-dir`` and ``--skip-file`` options. These can be specified more than once, and it accepts Unix-style wildcards.

::

    python3 manage.py dmp collectstatic --skip-dir=.cached_templates --skip-file=*.txt --skip-file=*.md

If you need to include additional directories or files, specify them with the ``--include`` option. This can be specified more than once, and it accepts Unix-style wildcards:

::

    python3 manage.py dmp collectstatic --include-dir=global-media --include-dir=global-styles --include-file=*.png

If you have ``rcssmin`` and ``rjsmin`` installed (via pip), DMP will minify your CSS and JS during the collection process.  If you are minifying with another tool (webpack, Google's minifier, etc.), disable minification with:

::

    python3 manage.py dmp collectstatic --no-minify
