coop-bar, configurable toolbar
===============================================

* `What is coop_bar good for?`_
* `Quick start`_

.. _What is coop_bar good for?: #good-for
.. _Quick start?: #quick-start

.. _good-for:

.. figure::  https://raw.github.com/quinode/coop-bar/master/docs/coop_bar.png
   :align:   left

What is coop_bar good for?
------------------------------------
django-coop is a set of several apps for building cooperative websites. It is based on Django.

Thses apps may need a to display an admin toolbar. For example, the CMS component may use this bar
to display edit, cancel and save links.

coop_bar tries to provide a simple and configurable mechanism

.. _quick-start:

Quick start
------------------------------------
In settings.py, add 'coop_bar' (with an underscore) to the INSTALLED_APPS
Under Django 1.3, the static folder should be found automatically, as the templates folder
In urls.py add (r'^coop_bar/', include('coop_bar.urls')) to your urlpatterns

For each app needing to add links to coop_bar, create a coop_bar_cfg.py file
In this file, add a load_commands function as follows ::

    from django.core.urlresolvers import reverse
    from django.utils.translation import ugettext as _

    def django_admin_command(request, context):
        if request and request.user.is_staff: #request might be None
            return u'<a href="{0}">{1}</a>'.format(reverse("admin:index"), _('Admin'))

    def load_commands(coop_bar):
        coop_bar.register_command(django_admin_command)


In load_commands, you can register as much callback functions as you want. A callback (django_admin_command in the previous example)
is just a function with request and context as args. It returns some html code to display in the bar or None.

In your base.html, add the following template tags::

    {% load coop_bar_tags %}
    <html>
    <head>
        ...
        {% coop_bar_headers %}
    </head>
    <body>
        {% coop_bar %}
        ...
        your page
        ...
        {% coop_bar_footer %}
    </body>


Humanized Messages
------------------------------------

.. figure::  https://raw.github.com/quinode/coop-bar/master/docs/humanmsg.png
   :align:   center

Coop-bar takes also care of providing a nice display of django session messages.
It's based on the `Humanized Message idea <http://code.google.com/p/humanmsg/>`_ , which displays a big fat sentence in the middle of the screen and fades away as soon as you move your mouse, or five seconds later.
You can add a Message log at the bottom of the web page by adding this to your django settings file::

DISPLAY_MESSAGES_LOG = True



License
=======

`Fugue icon set <http://p.yusukekamiyamane.com/>`_  by Yusuke Kamiyamane. All rights reserved. Licensed under a Creative Commons Attribution 3.0 License.

coop-bar itself uses the same license as Django (BSD).

coop-bar development was funded by `CREDIS <http://credis.org/>`_, FSE (European Social Fund) and Conseil Regional d'Auvergne.
