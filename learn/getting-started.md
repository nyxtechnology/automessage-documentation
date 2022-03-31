# Getting started

The purpose of this tutorial is to give a simple overview of **Automessage** ecosystem.

### Events Map

**Automessage** works with predefined actions and uses custom conditions to trigger these actions.

The event configuration is done in the `config/eventsMap.json` file. The event map is nothing more than a simple statement of which controller and method each event _**'calls'**_ based on the conditions you created. It should look like this:

```json
{
  "boardActions": [
    {
      "conditions": [
        {
          "post.header.x-forwarded-for": "178.128.181.251",
          "post.body.description": "act-completeChecklist",
          "post.body.user": "gilberto.souza"
        },
        {
          "post.body.from": "mysite.com"
        }
      ]
      "messageControllers": [
        {
          "controller": "App\\Http\\Controllers\\TelegramController",
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
          "controller": "App\\Http\\Controllers\\SendMailController",
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

Each of your events can have one or several **actions objects** (in our example there is only one). Each **actions object** is made up of _**conditions**_ and **message controllers**.

#### Conditions

There may be one, several or no conditions (_no condition means that any POST received by **Automessage** will trigger the actions of that **actions object**. We do not recommend creating events without condition_).

Each item of the **conditions** array is a **test object**. It means a group of test object to be validated so that the actions of this **actions object** can be performed.

Items of a **test object** are related to each other with _**`and`**_ logical operator, which means that all tests must be `true` for **test object** to be considered true.

**Test objects** are related to each other with _**`or`**_ logical operator, which means that at least one tests must be `true` for the **conditions** array to be considered true and actions to be performed.

Our example has two **test objects** in the **conditions** array (_line 5 to 9, line 10 to 12_).

Let's analyse the first **test object**. There are 3 conditions in it that must be equal to true. Go ahead to understand each one.

{% hint style="info" %}
Whenever we need to use a dynamic item, it must be sent in the POST **body** or we can also use POST **header** items. You can identify the items your system sends in the POST header and body using services like [webhook.site](https://webhook.site) or [`pipedream.com`](https://pipedream.com).
{% endhint %}

* The first condition (`"post.header.x-forwarded-for": "178.128.181.251"`). It means checking if the value of item received in `x-forwarded-for` from POST **header** is equal to `178.128.181.251`.
* The second condition (`"post.body.description": "act-completeChecklist"`). It means checking if the value of the item received in the `description` of the POST **body** is equal to `act-completeChecklist`.
* The third condition (`"post.body.user": "gilberto.souza"`). It means checking if the value of the item received in **user** from the POST **body** is equal to `gilberto.souza`.

The second **test object.** There is 1 condition in it that must be equal to true.

* The condition (`"post.body.from": "mysite.com"`). It means checking if the value of the item received in the `from` of the POST **body** is equal to `mysite.com`.

#### Message Controllers

There can be one or several **instructions objects** (_in our example we have 2 **instructions objects**. One started on line 10 and another on line 21)_.

Each **instructions object** has the path of the **controller** and the **methods** with their respective _**parameters**_. As the **controller** path is self-explanatory, let's pay attention to the **methods**.

We can have one or several **methods objects** for each **controller** and each **method object** is formed by the method **name** and a **parameters object**.

Taking as an example the `sendMail` **method object** that starts on _line 29_. It has fixed (_**to**_ and _**template**_) and dynamic parameters, such as **message** (_see line 33_ that will use the value of the item received in **text** from the POST **body**.

{% hint style="info" %}
**Attention!** The controllers and methods are performed in the order are add.
{% endhint %}

You are now ready to create your own events in `eventsMap.json` and determine which controllers and methods it will execute.

### Triggering an event

Configure your private webhook key in the `.env` file. This is more of a security measure, so your webhook url will be secret.

```
WEBHOOK_KEY=your-Key-secret-here
```

To trigger an event you can do programmatically sending a POST request to the `api/webhook/your-Key-secret-here route`. Or if you use a third-party system, just configure the _webhook_ of this system for the `api/webhook/your-Key-secret-here` route. You can send any JSON structure.
