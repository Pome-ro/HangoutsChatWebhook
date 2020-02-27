# Google Hangouts Chat Webhook Module

This module is designed to make it quick and easy to send messages via Google Hangouts Chat's Webhook feature. There are two kind of messages that can be sent via Webhook: Text and Cards

* **Text:** These messages are simple, they can include markdown style formatting for bold, italic, and strike-through styles. You can include line breaks as well.
* **Cards:** These messages are complex. They are constructed out of many items that build a Google style card with information. They can include inline images, can be broken up in to sections, and can contain hyperlinks. In the text, HTML is supported.

For more information on the types of widgets that can exist in a Card, check the [Hangouts Chat API Reference](https://developers.google.com/hangouts/chat/reference/message-formats/cards)

## Current Features
* Simple text message sending.
* Basic Card Functionality.

## Simple Text Example
Sending a simple text message is... simple! You can include line breaks and text formating using Hangout Chats markdown style syntax.

```powershell
Import-Module -name GHCWebhook
$URI = "https://chat.googleapis.com/..."

$Text = "*Alert! Please read.*
This is a sample message with formating"

$Message = New-GHCTextMessage -Message $text

Send-GHCWebhookMessage -URI $URI -Message $Message

```

## Card message example
Sending a Card message utlizes a number of cmdlets nested together to build the card of your liking. Here is a recreation of the JSON example on the [Google Hangouts Chat API documentation](https://developers.google.com/hangouts/chat/reference/message-formats/cards#full_example_pizza_bot).

![](/img/screenshot.png)

```powershell
$uri = "https://chat.googleapis.com/..."

$message = New-GHCCardMessage -Content {
    New-GHCCardHeader -Title "Pizza Bot Customer Support" -Subtitle "pizzabot@example.com" -ImageURL "https://goo.gl/aeDtrS" -ImageStyle avatar
    New-GHCCardSection -Content {
        New-GHCCardWidget -Content {
            New-GHCWidgetKeyValue -TopLabel "Order No." -Content "12345"
            New-GHCWidgetKeyValue -TopLabel "Status" -Content "In Delivery"
        }
        New-GHCCardWidget -Header "Location" -Content {
            New-GHCWidgetImage -URL "https://maps.googleapis.com/..."
        }
        New-GHCCardWidget -Content {
            New-GHCWidgetTextButton -text "OPEN ORDER" -onclickurl "https://example.com/orders/..."
        }
    }
}

Send-GHCWebhookMessage -URI $uri -Message $message
```
When creating a card message follow the naming convention. `New-GHCCard...` items nest under `New-GHCCardMessage`, `New-GHCWidget...` items nest under the `New-GHCCardWidget`. All `Widgets` must fall under a `Section` and every card message must have at least one `Section`.
