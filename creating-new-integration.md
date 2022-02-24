# Creating new integration

### Controller class

To create integration with a new service, the developer needs to create a controller class in `app\Http\Controllers` where he must do all the integration logic with the service he wants to integrate.

To make it clearer, let's take the integration with Telegram service as an example. The class below integrates with Telegram using the `telegram-bot/api` library.

```php
class TelegramController extends Controller
{
    private BotApi $telegram;

    /**
     * TelegramController constructor.
     */
    public function __construct(){
        $this->telegram = new BotApi(Config::get('telegram.api_key'));
    }

    /**
     * Send message to Telegram
     * @param $settings
     */
    public function sendMessage($settings){
        try {
            $this->telegram->sendMessage($settings['params']['to'], $settings['params']['message']);
        } catch (InvalidArgumentException|Exception $e) {
            Log::error('TelegramController->sendMessage() ' . $e->getMessage());
        }
    }
}
```

Let's pay attention to the `sendMessage` method that receives an array as a parameter, in this case named `$settings`. By default, every method that your event [_**'calls'**_](learn/getting-started.md#events-map) will receive an array with all the information received in the webhook. In this array the _params key_ will have as value all the information sent in the JSON [_metadata_](learn/getting-started.md#triggering-an-event) by POST so to access the _message_ for example just use `$settings['params']['message']`

### Mapping an event to your integration

The next step is to map an event that calls the `sendMessage` method of the `TelegramController` class. Access the `config/eventsMap.json` file and add your event. It must follow the existing pattern.

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
  },
  "sendTelegramAlert": {
    "classes": [
      {
        "name": "App\\Http\\Controllers\\TelegramController",
        "methods": [
          "sendMessage"
        ]
      }
    ]
  }
}
```

In this example, we mapped the `sendTelegramAlert` event that will call the `sendMessage` method of the `TelegramController` class we created previously.

### Triggering the new event

To trigger your event for **Automessage** you must POST to the `api/webhook` route. In our example, the `sendMessage` method of the `TelegramController` class only needs two information to send the message (**to** and **message**) so our JSON will look like this:

```json
{
  "event" : "sendTelegramAlert",
  "metadata" :
  {
    "message" : "Hello World!",
    "to" : "123456",
  }
}
```
