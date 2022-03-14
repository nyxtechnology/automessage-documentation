# Trigger test for sending email

Access POSTMAN or your favorite app for test.\
Change method to POST and send to the URL **'automessage.docker.local/api/webhook'**.\
JSON body to test send: \
\
{\
&#x20;   "event": "sendMailSMTP",\
&#x20;   "metadata":\
&#x20;       {\
&#x20;           "name": "name-for-test" (Example: Oliver Sykes, Marty McFly, Microsoft)\
&#x20;           "to": "email that will be sent"\
&#x20;           "subject": "email subject"\
&#x20;           "message": "message to be sent"\
&#x20;       }\
}\
\
Start the containers of automessage.\
Access the containers and start rabbitmq with '**php artisan queue:work'** command.\
Trigger the test email by postman or application of your choice
