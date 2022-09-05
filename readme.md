## Step - 1

Install the django all-auth package using below command.

pip install django-allauth

## Step - 2 

Add these settings in ur django project settings.py

INSTALLED_APPS = [
<!-- add these below apps to the existing one  -->
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

## step -3 
In urls.py, add this line
path('accounts/', include('allauth.urls'))

## step-4
Migrate the changes using below command.
python manage.py migrate

## step - 5
run the django service, if admin user is not created, pls create one using this cmd (python manage.py createsuperuser)
python manage.py runserver


## step - 6
Open admin dashboard in django 


