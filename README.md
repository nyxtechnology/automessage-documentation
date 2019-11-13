# Automessage Documentation

Documentation for developers to integrate their applications with the Automessage messaging system.


## Dependencies
Automessage is currently integrated with the following email marketing services:

[**Mailchimp**](https://mailchimp.com)<br>
**Api Key:** *API key by Mailchimp.*<br>
**List ID:** *A keyed and valued array of all your lists in use of Mailchimp.<br>
              - Key: A string with <b>no space</b> and <b>no uppercase</b> letters to identify your list.<br>
              - Value: Mailchimp list ID. Find [here](http://kb.mailchimp.com/lists/managing-subscribers/find-your-list-id) the ids.*<br>
```
[list_one => 1234abcd, list_two => efgh8765]
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
    "events": [
                {
                    "event_name": "event_one",
                    "actions": [
                                "scheduling_email,
                                "send_email_by_tamplate",
                               ],
                },
                {
                    "event_name": "event_two",
                    "actions": [
                                "cancel_scheduling_email,
                                "subscribe_list",
                               ],
                },
                {
                    "event_name": "event_three",
                    "actions": [
                                "update_member_subscribe_list,
                                "update_delivery_scheduling_email",
                               ],
                },
                {
                    "event_name": "event_four",
                    "actions": [
                                "cancel_all_scheduling_email,
                                "update_delivery_scheduling_email",
                               ],
                },
             ]
}
```
**event_name** <i>It is the name of your event, you can standardize as you prefer.</i><br>
**actions** <i>This is the set of actions your event must perform.</i><br>

 

 






