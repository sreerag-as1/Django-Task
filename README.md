# Django-Signals-And-Python-Custom-Class-Task

# Topic: Django Signals

Question 1: By default are django signals executed synchronously or asynchronously? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.

```
# views.py
from django.http import HttpResponse
from django.contrib.auth.models import User
from django.db.models.signals import post_save
from django.dispatch import receiver
import time

@receiver(post_save, sender=User)
def user_created_signal(sender, instance, created, **kwargs):
    if created:
        print("Signal handler started")
        time.sleep(3) 
        print("Signal handler completed")

def create_user_view(request):
    print("View started")
    user = User.objects.create(username="new_user")
    print("View finished")
    return HttpResponse("User created.")

```

Question 2: Do django signals run in the same thread as the caller? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.

```

# views.py
from django.http import HttpResponse
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.contrib.auth.models import User
import threading

@receiver(post_save, sender=User)
def user_created_signal(sender, instance, created, **kwargs):
    print(f"Signal handler thread: {threading.current_thread().name}")

def create_user_view(request):
    print(f"View thread: {threading.current_thread().name}")
    user = User.objects.create(username="new_user")
    return HttpResponse("User created.")
```

Question 3: By default do django signals run in the same database transaction as the caller? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.

```
# views.py

from django.http import HttpResponse
from django.contrib.auth.models import User
from django.db import transaction
from django.db.models.signals import post_save
from django.dispatch import receiver

@receiver(post_save, sender=User)
def user_created_signal(sender, instance, created, **kwargs):
    if created:
        print("Signal handler executed")
        raise Exception("Signal handler error")

def create_user_view(request):
    try:
        with transaction.atomic():
            user = User.objects.create(username="valid_username")
            print("User created")
    except Exception as e:
        return HttpResponse(f"Error: {str(e)}")

  return HttpResponse("User creation succeeded.")

```
# Topic: Custom Classes in Python

Description: You are tasked with creating a Rectangle class with the following requirements:

1. An instance of the Rectangle class requires length:int and width:int to be initialized.
2. We can iterate over an instance of the Rectangle class 
3. When an instance of the Rectangle class is iterated over, we first get its length in the format: {'length': <VALUE_OF_LENGTH>} followed by the width {width: <VALUE_OF_WIDTH>}

```
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def __iter__(self):
        yield {"length": self.length}
        yield {"width": self.width}

# Usage
rect = Rectangle(7, 4)

for dimension in rect:
    print(dimension)
```


