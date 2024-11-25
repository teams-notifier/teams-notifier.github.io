---
linkTitle: "User manual"
title: User manual
prev: /docs/addons
next: /docs/user/add-bot-to-groupchat
weight: 1000
---

Once the application is setup and available in MS Teams, the bot has to be invited group chat and channels to get `conversation_token`.
Each `conversation_token` is personal and tied to a user and a conversation (group chat or channel). 

Two different people will have two different token for the same group chat.

Those tokens, while attached, to a user are not Microsoft license dependent.

It is planned to be able to have several token per user/conversation and annotate them for the user to be able to request a token per app they need notification being sent to a given channel.

## Add the bot to a group chat

{{% steps %}}

### Step 1: Get bots
In a group chat, start a message with a `@`, wait for the menu to pop and select "Get bots"
{{< img src="0100-add-to-groupchat.png" width="300" style="margin-left: 0rem">}}

### Step 2: Invite Notifier
In the popup, search for **Notifier** and select it.
{{< img src="0110-add-app-to-group-chat.png" alt="add" width="300" style="margin-left: 0rem">}}

{{% /steps %}}

## Add the bot to a channel
{{% steps %}}

### Step 1: Get bots
{{< img src="0110-add-app-to-group-chat.png" width="300" style="margin-left: 0rem">}}

{{% /steps %}}

## Bot is already in the chat
