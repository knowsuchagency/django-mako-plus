Configuration Options
============================

DMP is configurable with options in your ``settings.py`` file.  The following are the defaults:

.. include:: ../django_mako_plus/defaults.py
   :literal:




``DEFAULT_APP``, ``DEFAULT_PAGE``
----------------------------------

When the url doesn't contain the app and/or page, such as ``http://www.yourserver.com/``, DMP uses the default app and page specified in your  settings.py variables: ``DEFAULT_APP`` and ``DEFAULT_PAGE``.

In the following table, the default app is set to ``homepage`` and your default page is set to ``index.html``:

+----------------------------------------------------------+-------------------+------------------------+-------------------------------------------+
| URL                                                      | App               | Page                   | Notes                                     |
+----------------------------------------------------------+-------------------+------------------------+-------------------------------------------+
| ``http://www.yourserver.com/``                           | ``homepage``      | ``index.html``         |                                           |
+----------------------------------------------------------+-------------------+------------------------+-------------------------------------------+
| ``http://www.yourserver.com/account/``                   | ``account``       | ``index.html``         |                                           |
+----------------------------------------------------------+-------------------+------------------------+-------------------------------------------+
| ``http://www.yourserver.com/login/``                     | ``login``         | ``index.html``         | If ``login`` **is** one of your apps      |
+----------------------------------------------------------+-------------------+------------------------+-------------------------------------------+
| ``http://www.yourserver.com/login/``                     | ``homepage``      | ``login.html``         | If ``login`` **is not** one of your apps  |
+----------------------------------------------------------+-------------------+------------------------+-------------------------------------------+
| ``http://www.yourserver.com/account/password/``          | ``account``       | ``password.html``      |                                           |
+----------------------------------------------------------+-------------------+------------------------+-------------------------------------------+



``APP_DISCOVERY``
-----------------------------

One of the primary purposes of DMP is to allow routing via convention instead of configuration.  At runtime, it does this by adding patterns for each of your apps to ``urls.py``.  By default, it adds patterns for each app contained in your main project directory (``BASE_DIR`` in settings).  It ignores (and doesn't route) Django's built-in apps and any third-party apps.

All Your Base
``````````````````````

If you want DMP to route *all* of your apps, set this option to ``"all"``.  If you do this, be sure to ``include()`` DMP's ``urls.py`` at the end of your ``urls.py``.  If DMP were to come first, it would override all the other url patterns.

No Discovery
``````````````````````

Set this option to ``"none"`` to disable DMP's patterns entirely.  DMP won't route anything by default this way, so your ``urls.py`` file will be untouched.  You should then register specific apps yourself by adding the following to ``someapp/apps.py``:

::

    from django.apps import AppConfig
    from django_mako_plus import register_dmp_app

    class SomeAppConfig(AppConfig):
        name = 'someapp'

        def ready(self):
            # explicitly tell DMP to route this app
            register_dmp_app(self)



``CONTEXT_PROCESSORS``
----------------------------

The context is the dictionary of variables sent into ``render()``.  When the context is created, it is run through several "processors" to add names like ``STATIC_URL``, ``request``, etc.

Read more about context processors in Django's `Template API documentation <https://docs.djangoproject.com/en/dev/ref/templates/api/#playing-with-context-objects>`_.


``TEMPLATES_CACHE_DIR``
---------------------------------

When a template is first rendered, the Mako engine generates a regular Python file out of it.  It is this generated Python file that actually runs to create your HTML.

This option sets the directory where these cached, generated files are located.  It is relative to the ``app/templates`` directory of each app.


``DEFAULT_TEMPLATE_ENCODING``
----------------------------------

DMP and the Mako engine will read your templates using this encoding.  Those who are coding their files with something other than ``utf-8`` should understand when and why it might need to be changed.

If you change this setting, be sure to run ``python manage.py dmp_cleanup`` so your cached templates get rebuilt.


``DEFAULT_TEMPLATE_IMPORTS``
--------------------------------

Python's standard libraries must be imported to be used within template code.  For example, to use the random module in a template, you'd place something like this at the top:

::
    <%! import random %>

    A random number is ${ random.randint(1, 100) }.

To automatically import the ``random`` module or any other Python module (including those from your project or installed via pip), add them in the ``DEFAULT_TEMPLATE_IMPORTS`` list.  When Mako renders your templates, it will include the imports automatically so you don't have repeat yourself in every template.

If you change this setting, be sure to run ``python manage.py dmp_cleanup`` so your cached templates get rebuilt.



``SIGNALS``
----------------------

This option sets whether DMP should trigger Django-style signals.  See `Signals <topics_signals.html>`_ for more information.


``CONTENT_PROVIDERS``
--------------------------

This is one of the primary functions of DMP.  It sets up autodiscovery of your static files, such as JS and CSS.  See `Static Files <static.html>`_ for more information on these options.


``WEBPACK_PROVIDERS``
------------------------

This option works in connection with ``python manage.py dmp_webpack``.  See ``Bundling <static_webpack.html>`_ for more information.


``TEMPLATES_DIRS``
-----------------------

In contrast with Django, DMP templates are closely connected to their apps.  If you need to search for templates in additional locations (a.k.a. the Django way), add folder paths to this list.  Django will search ``someapp/templates/`` first and then fall back to any directories you list here.


