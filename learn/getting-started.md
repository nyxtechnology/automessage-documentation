# Getting started

The purpose of this tutorial is to give a simple overview of **Automessage** ecosystem.

### Events Map

**Automessage** works with predefined actions and uses custom conditions to trigger these actions.

The event configuration is done in the `config/eventsMap.json` file. The event map is nothing more than a simple statement of which class and method each event _**'calls'**_ based on the conditions you created. It should look like this:

```json
{
  "boardActions": [
    {
      "conditions": {
        "post.header.x-forwarded-for": "178.128.181.251",
        "post.body.description": "act-completeChecklist",
        "post.body.user": "gilberto.souza"
      },
      "classes": [
        {
          "class": "App\\Http\\Controllers\\TelegramController",
          "methods": [
            {
              "sendMessage": {
                "to": "123456789",
                "message": "post.body.text"
              }
            }
          ]
        },
        {
          "class": "App\\Http\\Controllers\\SendMailController",
          "methods": [
            {
              "sendMail": {
                "to": "admin@nyx.tc",
                "subject": "post.body.description",
                "message": "post.body.text",
                "template": "movedCard",
                "templateVariables": {
                  "card": "post.body.card",
                  "user": "post.body.user"
                }
              }
            },
            {
              "sendMail": {
                "to": "gilberto.souza@nyx.tc",
                "subject": "post.body.description",
                "message": "post.body.text",
                "template": "youMovedCard"
              }
            }
          ]
        }
      ]
    }
  ]
}
```

Let's understand each item of this `eventsMap.json`.

Starting with the event **name** _(see line 2)_. Your event map can have one or several events, so the name is used to help organise and make it easier to identify each of your events.

Each of your events can have one or several **actions objects** (in our example there is only one). Each **actions object** is made up of _**conditions**_ and _**classes**_.

#### Conditions

There may be one, several or no conditions (_no condition means that any POST received by **Automessage** will trigger the actions of that **actions object**. We do not recommend creating events without condition_).

Each item of the **conditions** object means a test to be validated so that the actions of this **actions object** can be performed. It works with the logical operator (_**and**_) which means that all tests must be `true` for actions to be performed. In our example the 3 conditions must be equal to true. Let's analyse each of them and understand what they mean.

{% hint style="info" %}
Whenever we need to use a dynamic item, it must be sent in the POST **body** or we can also use POST **header** items. You can identify the items your system sends in the POST header and body using services like [webhook.site](https://webhook.site) or [`pipedream.com`](https://pipedream.com).
{% endhint %}

* The first condition (_see line 5_). It means checking if the value of item received in `x-forwarded-for` from POST **header** is equal to `178.128.181.251`.
* The second condition (_see line 6_). It means checking if the value of the item received in the `description` of the POST **body** is equal to `act-completeChecklist`.
* The third condition (_see line 7_). It means checking if the value of the item received in **user** from the POST **body** is equal to `gilberto.souza`.

#### Classes

There can be one or several **instructions objects** (_in our example we have 2 **instructions objects**. One started on line 10 and another on line 21)_.

Each **instructions object** has the path of the **class** and the **methods** with their respective _**parameters**_. As the **class** path is self-explanatory, let's pay attention to the **methods**.

We can have one or several **methods objects** for each **class** and each **method object** is formed by the method **name** and a **parameters object**.

Taking as an example the `sendMail` **method object** that starts on _line 24_. It has fixed (_**to**_ and _**template**_) and dynamic parameters, such as **message** (_see line 28_) that will use the value of the item received in **text** from the POST **body**.



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

text
