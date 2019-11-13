# Automessage Documentation

Documentation for developers to integrate their applications with the Automessage messaging system.


## Dependencies
Automessage is currently integrated with the following email marketing services:

[**Mailchimp**](https://mailchimp.com)<br>
* **Api Key:** *API key by Mailchimp.*<br>
* **List ID:** *A keyed and valued array of all your lists in use of Mailchimp.<br>
    * Key: A string with <b>no space</b> and <b>no uppercase</b> letters to identify your list.<br>
    * Value: Mailchimp list ID. Find [here](http://kb.mailchimp.com/lists/managing-subscribers/find-your-list-id) the ids.*<br>
```
{
  "lists":
  {
    "list_one": "1234abcd",
    "list_two": "efgh8765"
  }
}
```
[**Mandrill**](https://mandrill.com)<br>
**Api Key:** *API key by Mandrill.*<br>

[**Mailgun**](https://mailgun.com)<br>
**Domain:** *Domain name registered in Mailgun like a* <i><b>mg.something.com.</b></i><br>
**Api Key:** *Private API key by Mailgun.*<br>
**From Email Address:** *Address registred in Mailgun. All emails will be sent from this address.*<br>

## Event Creation
For now event creation needs to go through the Automessage development team. But dynamic event creation is already within our scope and will soon be released.<br>

We expect to receive the events to be created in the structure below:
```
{
  "events":
  [
    {
      "event_name": "event_one",
      "actions":
      [
        "scheduling_email",
        "send_email_by_tamplate"
      ]
    },
    {
      "event_name": "event_two",
      "actions":
      [
        "cancel_scheduling_email",
        "subscribe_list"
      ]
    },
    {
      "event_name": "event_three",
      "actions":
      [
        "update_member_subscribe_list",
        "update_delivery_scheduling_email"
      ]
    },
    {
      "event_name": "event_four",
      "actions":
      [
        "cancel_all_scheduling_email",
        "update_delivery_scheduling_email"
      ]
    }
  ]
}

```
**Attention! The actions are performed in the order received.**

**event_name** <i>It is the name of your event, you can standardize as you prefer.</i><br>
**actions** <i>This is the set of actions your event must perform. Each event can trigger one or several actions.</i><br>
We have the following actions available:
* <b>scheduling_email:</b> <i>Schedules the sending of an email. Below are the fields for this action to be performed, the required fields are with (*).</i>
    * <b>* externalId:</b> <i>This is an id that should be unique in your system. This field accepts a string.</i>
    * <b>* to:</b> <i>It is the recipient of the scheduled email. This field must be a valid email address.</i>
    * <b>* deliveryDate:</b> <i>This is the date the scheduled email should be sent. This field is given a string in the format YYYY-MM-DD.</i>
    * <b>* subject:</b> <i>This is the subject of the scheduled email. This field is given a string.</i>
    * <b>from:</b> <i>It is the sender of the scheduled email. This field must be a valid email.</i>
    * <b>fromName:</b> <i>This is the sender name of the scheduled email. This field is given a string.</i>
    * <b>toName:</b> <i>This is the name of the recipient of the scheduled email. This field is given a string.</i>
    * <b>template:</b> <i>It is the template that will be used for the scheduled email. This field is given a string.</i>
    * <b>templateVariables:</b> <i>These are the dynamic variables of the template used in the scheduled email. This field receives a key/value array.</i>
    * <b>eventStop:</b> <i>This is the event that cancels the sending of the scheduled email. This field is given a string.</i>    

Example:
```
{
  "extenalId" : "yor-project-checkout-a56fa78b-878f-41db-8518-2d2d842beAAs",
  "name" : "John Wick",
  "to" : "john-wick@wickmail.com",
  "deliveryDate" : "2020-11-17",
  "eventStop" : "complete-payment",
  "templateVariables" : 
  {
    "product" : "AK-47",
    "product-link" : "https://yor-project.com/product/a56fa78b-878f-41db-8518-2d2d842beAAs"
  },
  "template" : "abandoned-purchase",
  "subject" : "Your Ak-47 is still waiting for you"
}
```
