# Configure email sending system for different providers

### Gmail

#### Make the following changes to the **.env** file:

* **'MAIL\_HOST'** = smtp.gmail.com
* **'MAIL\_USERNAME'** = Use the same name as in **MAIL\_FROM\_NAME**
* **'MAIL\_FROM\_ADDRESS'** = youremail@gmail.com

### Yahoo

#### Make the following changes to the **.env** file:

* **'MAIL\_HOST'** = smtp.mail.yahoo.com
* **'MAIL\_USERNAME'** = youremail@yahoo.com
* **'MAIL\_FROM\_ADDRESS'** = youremail@yahoo.com

The yahoo have some more configurations needed to your operation:

1 - Access the 'account info' area.

2 - In the page that will be open go to 'account security'.

3 - If the two factor authentication is active, it will be necessary deactivate.

4 - Go to 'app password' and create a new password.

5 - After creating a new password, an alphabetical sequence will be shown as 'oeva oxpj ncfu ppfr'. Remove the spacing between the alphabetical sequence and added in 'MAIL\_PASSWORD'

Example: **'MAIL\_PASSWORD'** = **oevaoxpjncfuppfr**

### **Outlook**

**Make the following changes to the .env file:**

* **MAIL\_HOST** = smtp-mail.outlook.com
* **MAIL\_FROM\_ADDRESS** = youremail@outlook.com
* **MAIL\_USERNAME** = yourmeail@outlook.com&#x20;

