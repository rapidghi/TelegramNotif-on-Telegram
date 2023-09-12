# GLPI to Telegram Ticket Notifier

This PHP script allows you to monitor GLPI (an open-source ITSM tool) for new tickets and send notifications to a Telegram channel. It utilizes the GLPI API for ticket retrieval and the Telegram API for notifications.

## Requirements

- PHP
- cURL extension enabled
- GLPI API access (with username, password, and application token)
- Telegram bot with a valid token and a channel/chat ID

## Configuration

1. Replace the placeholders in the script with your actual values:

```php
$base_url = "http://glpi.example.com/apirest.php"; // GLPI API base URL
$username = "your_username"; // Your GLPI username
$password = "your_password"; // Your GLPI password
$app_token = "your_application_token"; // Your GLPI application token
$bot_token = "your_bot_token"; // Your Telegram bot token
$chat_id = "your_chat_id"; // Your Telegram channel/chat ID
```

2. Set up the GLPI API and Telegram bot as per your requirements.

## Usage

1. Run the script in a PHP environment. You can use a web server or command-line PHP.

```bash
php glpi_to_telegram.php
```

2. The script will check for new tickets in GLPI. When it detects a new ticket, it will send a notification to the specified Telegram channel.

## Additional Notes

- The script uses cURL to interact with GLPI and Telegram APIs, ensure the cURL extension is enabled in your PHP environment.
- Make sure the GLPI API is accessible from the environment where the script is executed.
- Ensure proper security measures are in place, such as encryption and secure storage of credentials.

## License

This script is provided under the [MIT License](LICENSE). Feel free to modify and distribute it as per your requirements.

## Disclaimer

This script is provided as-is without any warranty. Use it at your own risk. The authors and contributors are not responsible for any damages or liabilities arising from the use of this script.
