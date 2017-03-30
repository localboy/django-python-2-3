# django-python-2-3
Making your Djabgo project compatible with both Python versions consists of the following steps:

1. Add `from __future__ import unicode_literals` at the top of each module
and then use usual quotes without a `u` prefix for Unicode strings and a `b` prefix for
bytestrings.
2. To ensure that a value is bytestring, use the `django.utils.encoding.smart_bytes` function. To ensure that a value is Unicode, use the `django.utils.encoding.smart_text` or `django.utils.encoding.force_text` function.
3. In your models use `__str__` method instead of `__unicode__` and add the `python_2_unicode_compatible` decorator
4.
    ```python
    # models.py
    # -*- coding: UTF-8 -*-
    from __future__ import unicode_literals
    from django.db import models
    from django.utils.translation import ugettext_lazy as _
    from django.utils.encoding import python_2_unicode_compatible

    @python_2_unicode_compatible
    class NewsArticle(models.Model):
        title = models.CharField(_("Title"), max_length=200)
        content = models.TextField(_("Content"))

        def __str__(self):
            return self.title

        class Meta:
            verbose_name = _("News Article")
            verbose_name_plural = _("News Articles")
    ```
4. To iterate through dictionaries, use `iteritems()` , `iterkeys()` , and
`itervalues()` from `django.utils.six` . Take a look at the following:

    ```python
    from django.utils.six import iteritems

    d = {"imported": 25, "skipped": 12, "deleted": 3}
    for k, v in iteritems(d):
        print("{0}: {1}".format(k, v))
    ```
5. At the time of capturing exceptions, use the `as` keyword, as follows:

    ```python
    try:
        article = NewsArticle.objects.get(slug="hello-world")
    except NewsArticle.DoesNotExist as exc:
        pass
    except NewsArticle.MultipleObjectsReturned as exc:
        pass
    ```
6. Use django.utils.six to check the type of a value as shown in the following:
    ```python
    from django.utils import six

    isinstance(val, six.string_types) # previously basestring
    isinstance(val, six.text_type) # previously unicode
    isinstance(val, bytes) # previously str
    isinstance(val, six.integer_types) # previously (int, long)
    ```
7. Use range from django.utils.six.moves ,Instead of xrange , as follows:
    ```python
    from django.utils.six.moves import range

    for i in range(1, 11):
        print(i)
    ```