Step 1:

To install django-otp, open up your terminal and type in the following command:

pip install django-otp qrcode
Step 2:

Next, you want to configure 2FA, and to do this we need to add the required django-otp configurations: ‘django_otp’ and ‘django_otp.plugins.otp_totp’

# settings.py

INSTALLED_APPS = [
   'django_otp',
   'django_otp.plugins.otp_totp',
]
Step 3:

Next, you want to add ‘django_otp.middleware.OTPMiddleware’ to our middleware.

# settings.py

MIDDLEWARE = [
   'django_otp.middleware.OTPMiddleware',
]
Step 4:

Add the following code before your urls.py patterns list:

# urls.py

from django.contrib.auth.models import User

from django_otp.admin import OTPAdminSite
from django_otp.plugins.otp_totp.models import TOTPDevice
from django_otp.plugins.otp_totp.admin import TOTPDeviceAdmin
Step 5:

Next, you will need to create an OTP admin class so that you can register the user and TOTPDevice model in Django’s administration/admin panel.

# urls.py

class OTPAdmin(OTPAdminSite):
   pass

admin_site = OTPAdmin(name='OTPAdmin')
admin_site.register(User)
admin_site.register(TOTPDevice, TOTPDeviceAdmin)
Step 6:

Create the necessary tables in your database for django-otp:

python manage.py migrate
Create a superuser to login to django admin:

python manage.py createsuperuser
Run your server to see the changes:

python manage.py runserver 
Step 7:

Head to the django admin panel via the following URL:

http://localhost:8000/admin
Then proceed to log in with your recently created superuser (admin) credentials.
