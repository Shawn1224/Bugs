## Django 1.11.x is not compatible with Python 3.7.
**Get `SyntaxError: Generator expression must be parenthesized` while run `python manage.py runserver`**
```shell
python manage.py runserver
Unhandled exception in thread started by <function check_errors.<locals>.wrapper at 0x7f813ebac290>
Traceback (most recent call last):
  File "/opt/anaconda3/lib/python3.7/site-packages/django/utils/autoreload.py", line 228, in wrapper
    fn(*args, **kwargs)
  File "/opt/anaconda3/lib/python3.7/site-packages/django/core/management/commands/runserver.py", line 117, in inner_run
    autoreload.raise_last_exception()
  File "/opt/anaconda3/lib/python3.7/site-packages/django/utils/autoreload.py", line 251, in raise_last_exception
    six.reraise(*_exception)
  File "/opt/anaconda3/lib/python3.7/site-packages/django/utils/six.py", line 685, in reraise
    raise value.with_traceback(tb)
  File "/opt/anaconda3/lib/python3.7/site-packages/django/utils/autoreload.py", line 228, in wrapper
    fn(*args, **kwargs)
  File "/opt/anaconda3/lib/python3.7/site-packages/django/__init__.py", line 27, in setup
    apps.populate(settings.INSTALLED_APPS)
  File "/opt/anaconda3/lib/python3.7/site-packages/django/apps/registry.py", line 85, in populate
    app_config = AppConfig.create(entry)
  File "/opt/anaconda3/lib/python3.7/site-packages/django/apps/config.py", line 94, in create
    module = import_module(entry)
  File "/opt/anaconda3/lib/python3.7/importlib/__init__.py", line 127, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1006, in _gcd_import
  File "<frozen importlib._bootstrap>", line 983, in _find_and_load
  File "<frozen importlib._bootstrap>", line 967, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 677, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 728, in exec_module
  File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
  File "/opt/anaconda3/lib/python3.7/site-packages/django/contrib/admin/__init__.py", line 4, in <module>
    from django.contrib.admin.filters import (
  File "/opt/anaconda3/lib/python3.7/site-packages/django/contrib/admin/filters.py", line 10, in <module>
    from django.contrib.admin.options import IncorrectLookupParameters
  File "/opt/anaconda3/lib/python3.7/site-packages/django/contrib/admin/options.py", line 12, in <module>
    from django.contrib.admin import helpers, widgets
  File "/opt/anaconda3/lib/python3.7/site-packages/django/contrib/admin/widgets.py", line 151
    '%s=%s' % (k, v) for k, v in params.items(),
    ^
SyntaxError: Generator expression must be parenthesized
```

Solution

https://stackoverflow.com/questions/48822571/syntaxerror-generator-expression-must-be-parenthezised-python-manage-py-migra
