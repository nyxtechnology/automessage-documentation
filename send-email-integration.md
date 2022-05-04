# Send Email integration

You need to do the following steps to configure your automessage testing environment.

## Setting the .env file

Do the following changes in .env file:
In 'MAIL_DRIVER' add the type of your email provider.
Ex: IMAP or SMTP.
In 'MAIL_HOST' add the server of your email provider.
Ex: smtp.gmail.com.
In 'MAIL_PORT' add the port of your email provider.
Ex: 587 or 465.
In 'MAIL_USERNAME' add the email address that will be used.
In 'MAIL_PASSWORD' add the password of the email that will be used.
In 'MAIL_ENCRYPTION' add the type of cryptography with your email provider recommend.
In 'MAIL_FROM_ADDRESS' add the email that will be used
In 'MAIL_FROM_NAME' add the name that will be used to identify your email.
Ex: Ubisoft, PlayStation, Apple.

If you have some doubts about specific settings of your email provider, search for how to configure your email provider with SMTP or IMAP.

## Trigger test for sending email

Start the containers of automessage in your machine.
Access the containers and start rabbitmq with **'php artisan queue:work'** command.
Access POSTMAN or your favorite app for test.
Change send method to POST and send to the following URL **'automessage.docker.local/api/webhook'**.
This is an example of JSON body to test the email receive

{
  "event": "sendMailSMTP",
  "metadata":
  {
    "name": "name-for-test" (Example: Oliver Sykes, Marty McFly, Microsoft)
    "to": "email that will be sent"
    "subject": "email subject"
    "message": "message to be sent"
  }
}

Trigger the test email by postman or application of your choice.