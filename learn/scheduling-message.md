# Scheduling Message

Automessage's message scheduling system works for all our integrations, for a better understanding of how scheduling works let's use the `config/eventsMap.json` example below.

```json
{
  "PaymentActions": [
    {
      "conditions": {
        "post.header.from": "https://marketing.place.nyx.tc",
        "post.body.event": "startPayment"
      },
      "classes": [
        {
          "class": "App\\Http\\Controllers\\SchedulingController",
          "methods": [
            {
              "messageScheduling": {
                "dateDelivery": {
                  "startDate": "07/03/2022",
                  "operation": "+ 3 days"
                },
                "waysDelivery": [
                  {
                    "class": "App\\Http\\Controllers\\TelegramController",
                    "methods": [
                      {
                        "sendMessage": {
                          "to": "post.body.to",
                          "message": "post.body.text"
                        }
                      }
                    ]
                  }
                ],
                "conditionsStopDelivery": {
                  "post.body.cardId": "wwutm7rsiJPX5ar3z",
                  "post.header.x-forwarded-for": "178.128.181.251",
                  "post.body.description": "act-moveCard"
                }
              }
            }
          ]
        }
      ]
    }
  ]
}
```

To schedule a message, your event map must be configured to call the `messageScheduling` method of the `SchedulingController` class as you can see in line 13.

The `messageScheduling` method receives as mandatory parameters `dateDelivery` (see line 14) and `waysDelivery` (see line 18). It also accepts the optional `conditionsStopDelivery` parameter (see line 31).

#### Date Delivery

The `dateDelivery` parameter is an object that must contain 2 variables. This works by calculating the delivery date of the message according to the parameters sent. In our example above the delivery date would be **03/10/2022** because the base date `"startDate"` is 03/07/2022 and the `"operation"` is "+ 3 days" indicates to add three days to the base date.

* **startDate** (see line 15) is the base date of the schedule and a valid date is expected as a value, such as "12/31/2022".
* **operation** (see line 16) is the calculation made on the `startDate` and the expected values ​​are
  * **operator** (+ or -).
  * **quantity** (an integer).
  * **type** (days, weeks, months or years)
  * Values ​​are separated by **single space** and all 3 are required such as "- 1 weeks", "- 3 days", "+ 1 months", "+ 2 years" ...

#### Ways Delivery

It's how the message should be delivered. You can see that the `JSON` configuration follows the same principle as the [classes](getting-started.md#classes).

#### Conditions Stop Delivery

If your schedule message needs to be cancelled, this is where you set the stop conditions. This works the same as [conditions](getting-started.md#conditions). Whenever **Automessage** receives a post on its webhook, it checks if that post meets the stop conditions of some schedule message.
