Show 404 page on django when DEBUG=True
#######################################

:date: 2015-04-29 10:20
:tags: django, debug, 404, error, python
:category: django
:slug: django-show-404-page
:author: Serafeim Papastefanos
:summary: How to display the 404 error page on django when DEBUG=True

The default 404 error page on django can be `easily overriden`_ by adding
a template named ``404.html`` to the top level directory of your templates.
However, on your development environment you'll never be able to see this
template because when ``DEBUG=True`` django will render the debug not found
page to help you debug your url configuration.

If you want to display that page in your development environment you can always
change the DEBUG setting to False, however there's a better way: Add a url
pattern for django's default 404 view - just  add the following to your ``urls.py``:

.. code-block:: python

  import django.views.defaults

  urlpatterns = patterns('',
      # Other url patterns ...
      url(r'^404/$', django.views.defaults.page_not_found, ),
  )


You'll then be able to see your 404 page by visiting the defined URL!

**Upgrade for newer Django versions**

I've recently found out that for newer Django versions the ``page_not_found`` view needs a second
parameter (beyond ``request``) named ``exception`` (`see here`_). Thus
if you just add the view to your urls as I propose you'll get an exception. To fix it, you can do something like:

.. code-block:: python

  import django

  def custom_page_not_found(request):
      return django.views.defaults.page_not_found(request, None)

  urlpatterns = [
      path("404/", custom_page_not_found),
  ]

This creates a simple view that calls the builtin ``page_not_found`` passing it ``None`` as the ``exception`` parameter.


.. _`easily overriden`: https://docs.djangoproject.com/en/1.8/topics/http/views/#the-http404-exception
.. _`see here`: https://docs.djangoproject.com/el/2.1/ref/views/#django.views.defaults.page_not_found
