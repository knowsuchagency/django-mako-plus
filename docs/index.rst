.. toctree::
    :maxdepth: 2
    :hidden:

    about
    installation
    tutorial
    topics


Django-Mako-Plus
================================================

Routing Django to Mako since 2013.


Use If You've Said...
--------------------------

-  Why are Django templates weak sauce? Why not just use regular Python
   in templates?
-  Why does Django make me list every. single. page. in urls.py?
-  Is there a better way to connect my HTML files to related CSS and JS files?
-  Can I include Python code in my CSS and Javascript files.



Quick Start
----------------------

.. code:: bash

    # install django, mako, and DMP
    pip3 install django mako django-mako-plus

    # create a new project with a 'homepage' app
    python3 -m django startproject --template=http://cdn.rawgit.com/doconix/django-mako-plus/master/project_template.zip mysite
    cd mysite
    python3 manage.py startapp --template=http://cdn.rawgit.com/doconix/django-mako-plus/master/app_template.zip --extension=py,htm,html homepage

    # open mysite/settings.py and append 'homepage' to the INSTALLED_APPS list
    INSTALLED_APPS = [
        ...
        'homepage',
    ]

    # run initial migrations and run the server
    python3 manage.py migrate
    python3 manage.py runserver

    # Open a browser to http://localhost:8000/

Note that on Windows, ``python3`` is ``python`` and ``pip3`` is ``pip``. Python 3+ is required.



Compatability
=================

DMP works with Python 3.4+ and Django 1.9+.

DMP can be used alongside regular Django templates, Jinja2 templates, and other third-party apps (including embedding these other tags within DMP templates when needed). It plugs in via the regular ``urls.py`` mechanism, just like any other view.

Be assured that it plays nicely with the other children.
