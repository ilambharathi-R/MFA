# Django Admin Multi-Factor Authentication (MFA) Setup Guide

This guide provides step-by-step instructions to configure Multi-Factor Authentication (MFA) using One-Time Passwords (OTP) for your Django admin page using the django-otp package.

## Installation

1. Install django-otp and qrcode using pip:

    ```bash
    pip install django-otp qrcode
    ```

## Configuration

2. Add 'django_otp' and 'django_otp.plugins.otp_totp' to your INSTALLED_APPS in settings.py:

    ```python
    INSTALLED_APPS = [
        ...
        'django_otp',
        'django_otp.plugins.otp_totp',
    ]
    ```

3. Add 'django_otp.middleware.OTPMiddleware' to your MIDDLEWARE in settings.py:

    ```python
    MIDDLEWARE = [
        ...
        'django_otp.middleware.OTPMiddleware',
    ]
    ```

4. Import necessary modules and classes in your urls.py:

    ```python
    from django.contrib.auth.models import User
    from django_otp.admin import OTPAdminSite
    from django_otp.plugins.otp_totp.models import TOTPDevice
    from django_otp.plugins.otp_totp.admin import TOTPDeviceAdmin
    ```

5. Create an OTP admin class and register the User and TOTPDevice models:

    ```python
    class OTPAdmin(OTPAdminSite):
        pass

    admin_site = OTPAdmin(name='OTPAdmin')
    admin_site.register(User)
    admin_site.register(TOTPDevice, TOTPDeviceAdmin)
    ```

## Database Setup

6. Create the necessary tables in your database for django-otp:

    ```bash
    python manage.py migrate
    ```

7. Create a superuser to access the Django admin:

    ```bash
    python manage.py createsuperuser
    ```

## Run Server

8. Run your server:

    ```bash
    python manage.py runserver
    ```

## Access Django Admin

9. Open your web browser and go to the Django admin panel URL:

    [http://localhost:8000/admin](http://localhost:8000/admin)

   Log in with the superuser credentials you created earlier.

## Enable Two-Factor Authentication (2FA)

10. To enable 2FA:

    - Navigate to the Django admin panel.
    - Add a new TOTP device and set the tolerance level.
    - Update your timezone in settings.py.
    - Scan the QR code with Google Authenticator.

## Update Admin URL

11. Update the admin URL in urls.py:

    ```python
    from .urls import admin_site

    urlpatterns = [
        path('admin/', admin_site.urls),
    ]
    ```

Replace the default admin URL with the provided code in your urls.py file.

## Step 10: Test 2FA

Test the 2FA by logging into Django admin using Google Authenticator.

For further assistance or inquiries, please refer to the official Django documentation or contact our support team.

