
# Django and keycloak integration using all-auth

This below steps will help you to integrate between the django and keycloak using django all-auth.


These are the steps to be followed for integration

### step - 1

Install the django all-auth package using below command.

```pip install django-allauth```

### step - 2

Add these settings in ur django project settings.py


```
INSTALLED_APPS = [
     # add these below apps to the existing one
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'allauth.socialaccount.providers.keycloak',
]


SITE_ID = 1

# Specify the context processors as follows:
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                # Already defined Django-related contexts here

                # `allauth` needs this from django
                'django.template.context_processors.request',
            ],
        },
    },
]


AUTHENTICATION_BACKENDS = [
    ...
    # Needed to login by username in Django admin, regardless of `allauth`
    'django.contrib.auth.backends.ModelBackend',

    # `allauth` specific authentication methods, such as login by e-mail
    'allauth.account.auth_backends.AuthenticationBackend',
    ...
]

SOCIALACCOUNT_PROVIDERS = {
    'keycloak': {
        'KEYCLOAK_URL': 'http://<keycloak_ip>:<port_no>/auth',
        'KEYCLOAK_REALM': '<realm_name>'
    }
}
```


### step - 3 

In urls.py, add this line

```path('accounts/', include('allauth.urls'))```

### step - 4

Migrate the changes using below command.

```python manage.py migrate```

### step - 5

run the django service

```python manage.py runserver```

If admin user is not created, pls create one using this cmd 

```python manage.py createsuperuser```

### step - 6

Open keycloak master realm 

i. Go to clients

ii. Create client

While creating client speicify the Valid redirect URIs as 

```http://<django_service_ip>:<port_no>/accounts/keycloak/login/callback/```

from this save the client id and secret key(If you dont know how to create a client kindly the refer the official doc of keycloak).

### step - 7

Open Django admin dashboard

i. Go to social applications.

ii. Create a social application.

iii. Fill the form, select provider as keycloak, give the client id and secret key from keycloak saved one.
 
iv. In Site field keep as example.com


Save the form :)


### step - 8 

Open browser and open below link 

```http://<django_service_ip>:<port_no>/accounts/login/```

### step - 9 

You will be seing login page alongside with keycloak login option click on it.

It will redirect to keycloak sign in page after successfull sign in it will redirect to django urls.

Note: Make sure you have users in keycloak :)

### step - 10 

peace :)






