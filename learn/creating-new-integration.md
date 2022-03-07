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
            $this->telegram->sendMessage($settings['to'], $settings['message']);
        } catch (InvalidArgumentException|Exception $e) {
            Log::error('TelegramController->sendMessage() ' . $e->getMessage());
        }
    }
}
```

Let's pay attention to the `sendMessage` method that receives an array as a parameter, in this case named `$settings`. This parameter must be informed in the `config/eventsMap.json` (learn more [here](getting-started.md#events-map))

### Mapping an event to your integration

The next step is to map an event that calls the `sendMessage` method of the `TelegramController` class. Access the `config/eventsMap.json` file and add your event. It must follow the existing pattern.

```json
{
  "boardActions": [
    {
      "conditions": {
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
        }
      ]
    }
  ]
}
```

In this example, we mapped the `boardActions` event that will call the `sendMessage` method of the `TelegramController` class we created previously.

### Triggering the new event

To trigger an event see [here](getting-started.md#triggering-an-event). Of course you can send any JSON structure but in this case we are waiting for some information in the body of the POST for example.

```json
{
  "text": "any text here",
  "cardId": "wwrsiJPar3z",
  "listId": "qmHZX4edoy",
  "boardId": "qmHZes",
  "user": "gilberto.souza",
  "description": "act-completeChecklist"
}
```
