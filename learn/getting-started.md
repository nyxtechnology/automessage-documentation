# Getting Started

The purpose of this tutorial is to give a simple overview of **Automessage** ecosystem.

### Events Map

**Automessage** works with predefined actions and uses custom events to trigger these actions.

The event configuration is done in the `config/eventsMap.json` file. The event map is nothing more than a simple statement of which class and method each event _**'calls'**_ it should look like this:

```json
{
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
  "event" : "startPayment",
  "metadata" :
  {
    "extenalId" : "your-project-checkout-a56fa78b-878f-41db-8518-2d2d842beAAs",
    "name" : "John Wick",
    "to" : "john-wick@wickmail.com",
    "deliveryDate" : "2020-11-17",
    "eventStop" : "paymentCompleted",
    "templateVariables" : 
    {
      "product" : "AK-47",
      "product-link" : "https://your-project.com/product/a56fa78b-878f-41db-8518-2d2d842beAAs"
    },
    "template" : "abandoned-purchase",
    "subject" : "Your Ak-47 is still waiting for you"
  }
}
```

