# django-python-2-3
Making your Djabgo project compatible with both Python versions consists of the following steps:

1. Add from __future__ import unicode_literals at the top of each module
and then use usual quotes without a u prefix for Unicode strings and a b prefix for
bytestrings.

2. To ensure that a value is bytestring, use the django.utils.encoding.smart_
bytes function. To ensure that a value is Unicode, use the django.utils.
encoding.smart_text or django.utils.encoding.force_text function.
