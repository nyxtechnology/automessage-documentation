# Telegram integration

Integration with the [Telegram](https://telegram.org) service has already been implemented, however, it is necessary to perform some individual configurations for the service to work properly.

#### First things first

You have to create your **bot** to do this you should follow the Telegram documentation [here](https://core.telegram.org/bots#6-botfather). When you finish creating your bot you will receive an authorization token. This token must be entered in the `.env` file in the variable `TELEGRAM_BOT_TOKEN`&#x20;

```
TELEGRAM_BOT_TOKEN=31342410:AAHtVjbT3OolC_Y-hH_mAwalllpwrioc
```

Now you need to configure your bot's communication `webhook`, for that use your browser and access `https://api.telegram.org/bot`**`31342410:AAHtVjbT3OolC_Y-hH_mAwalllpwrioc`**`/setWebhook?url=https://`**`your-automessage-url-base`**`/api/telegram` changing the values according to your environment.

Open Telegram and search for the bot you created. Start chatting with the bot! (to avoid spam telegram bots cannot start a chat).

If everything is correct, when using the `/start` command in the chat with your bot it should return the chat id, save the id because it is necessary to trigger the events for **Automessage**.

#### Using

Now that you have the chat id, just make a POST request to **Automessage**, sending the _chat id_ (**to**) and the _message_ (**message**) in the JSON `metadata`.

```json
  "event" : "sendTelegramAlert",
  "metadata" :
  {
    "message" : "Hello World!",
    "to" : "123456",
  }
}
```

#### Tip of the day

The Telegram integration controller [class](creating-new-integration.md#controller-class) has the `receiveMessage` method which, as its name suggests, is responsible for receiving messages sent to your bot. You can use this method to improve your bot interaction just by implementing more responses in this method.
