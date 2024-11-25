---
title: Get a token
weight: 1500
---

{{% steps %}}

### Step 1: Highligh the Bot
In the conversation for which you want a token, highlight the bot by posting a message with `@notifier`

{{< tabs items="in a Group Chat,in a Team" >}}

{{< tab >}}
In the message input, start typing `@notifier`, select the proposition, your typed text should be converted in purple text then post the message
{{< img src="groupchat-highlight-bot.png" style="margin-left: 0rem">}}
{{< /tab >}}

{{< tab >}}
In the message part of the editor (not the subject), start typing `@notifier`, select the proposition, your typed text should be converted in purple text then post the message
{{< img src="channel-highlight-bot.png" style="margin-left: 0rem">}}
{{< /tab >}}
{{< /tabs >}}

### Step 2: Get a token
The bot will reply with a card, click the card or the *Get a token* button

{{< tabs items="in a Group Chat,in a Team" >}}

{{< tab >}}
{{< img src="0010-get-token.png" style="margin-left: 0rem">}}
{{< /tab >}}

{{< tab >}}
{{< img src="0070-get-a-token.png" style="margin-left: 0rem">}}
{{< /tab >}}
{{< /tabs >}}

### Step 3: Check your private message
The bot will send you a private message with the token corresponding to the conversation you clicked the card in.

{{< tabs items="for a named Group Chat,for an unnamed Group Chat,for a Team,for a Channel" >}}

{{< tab >}}
{{< img src="0020-groupchat.png" style="margin-left: 0rem">}}
{{< /tab >}}

{{< tab >}}
{{< img src="0030-unnamed-groupchat.png" style="margin-left: 0rem">}}
{{< /tab >}}

{{< tab >}}
{{< img src="0130-get-team-channel-token.png" style="margin-left: 0rem">}}
{{< /tab >}}

{{< tab >}}
{{< img src="0130-get-team-channel-not-main-token.png" style="margin-left: 0rem">}}
{{< /tab >}}

{{< /tabs >}}


### Step 4: Use the token

Use this `conversation_token` with the *activity-api* or an application that uses it

As a reminder, the documentation of the *activity-api* is available in the service itself `http://activity-api/docs`.

You'll find a copy of it here: [activity-api.json](openapi-activity-api.json) or [activity-api.yaml](openapi-activity-api.yaml)

{{% /steps %}}
