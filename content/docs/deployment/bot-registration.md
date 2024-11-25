---
title: Bot registration
prev: /docs/deployment/
next: /docs/deployment/core
#next: /docs/core/bf-directline-endpoint
weight: 20
---

You'll need:
* some elevated privileges
* a square icon of 192px by 192px
* an outline icon in 32px by 32px.

Registering a bot will bring you to 4 different MS portals, follow those steps.

## Bot creation and registration

* Go to https://dev.teams.microsoft.com/bots
* Create a bot
  * Give it a name

In the Bot config itself:
* Save its ID for later (for the Entra app and the bot framework portal)
* Configure the **Endpoint address** like https://bf-endbpoint-api.example.org/api/messages (where you deployed this api)
* Go to the bot framework portal: https://dev.botframework.com/bots/settings?id=ID_OF_YOUR_BOT
  * Update the icon
  * the Messaging endpoint should be set, if not, you can define it here
  * then click on *Manage Microsoft App ID and password*, this will send you to Entra ID's App registrations config of the bot

## Credential management

In the Entra App registration:
* Expand *Manage* and choose *Certificates & secrets*
* Choose either:
  * to generate a client secret (24 months max validity using the UI, longer using powershell)
  * generate and upload a certificate


## MS Teams app creation and publication

* Go to https://dev.teams.microsoft.com/apps
  * Give it a name

In the newly created app:
* **Basic information**
  * fill the required fields
  * in **Application (client) ID**, set the bot ID from the first step

* **Branding**: setup the Color icon, the Outline icon and the Accent color (for the provided icons, I suggest #5c61ce as accent color)

* **App features**: select Bot, then
  * choose the bot you created in the previous steps
  * select all the scopes (Personal, Team, Group Chat)
  * you can add a bot command
    * command: help
    * description: Show help
    * scope: Personal
  * Save

Once everything is ready, head up to *Publish* > *Publish to org* and click *Publish your app*, if submission fails, double check all mandatory fields are filled in *Configure* > *Basic information*


Now it has been published, it can take a bit for the app to show up on Teams Admin center.
* Go to https://admin.teams.microsoft.com/
* *Teams apps* > *Manage apps*
* Search in the list for your app name (let's say Notifier), it should be in **Blocked** status
* Click it and at the top click **Publish**


NOTE: You'll need to revalidate every time you publish an update of the app (icon, description, commands etc...)