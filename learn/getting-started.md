# Getting Started

The purpose of this tutorial is to give a simple overview of **Automessage** ecosystem.

### Events Map

**Automessage** works with predefined actions and uses custom events to trigger these actions.

The event configuration is done in the `eventsMap.json` file and it should look like this:

```json
{
  "sendTelegramAlert": {
    "classes": [
      {
        "name": "App\\Http\\Controllers\\TelegramController",
        "methods": [
          "sendMessage"
        ]
      }
    ]
  },
  "startPayment": {
    "classes": [
      {
        "name": "App\\Http\\Controllers\\SchedulingController",
        "methods": [
          "deleteSchedulingEmails",
          "saveSchedulingEmails"
        ]
      },
      {
        "name": "App\\Http\\Controllers\\MailController",
        "methods": [
          "senMails"
        ]
      }
    ]
  }
}

```

{% hint style="info" %}
**Attention!** The classes and methods are performed in the order are add.
{% endhint %}

You can create your own events in `eventsMap.json` and determine which classes and methods it will execute.

### Triggering an event

To trigger an event you need to send a POST to the `api/webhook` route. JSON structure that you must send in the POST is simple and follows the model below:

```json
{
  "event" : "sendTelegramAlert",
  "metadata" :
  {
    "to" : "123123",
    "message" : "Hello World!"
  }
}
```
