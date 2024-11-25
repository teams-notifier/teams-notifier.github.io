---
title: BotFramework Direct Line endpoint
weight: 30
---

*Teams Notifier's bot framework directline component*

homepage: https://github.com/teams-notifier/bf-directline-endpoint

## Intro

This service will be publicly available and receive messages from BotFramework about user interactions. It'll also generates the `conversation_token` accordingly and send related messages. It's main focus is user interaction.

Initialization schema is in `db/schema.sql`.
Migration and further improvements will be handled by `dbmate`

## Authentication

Authentication is either relying on MICROSOFT_APP_PASSWORD or MICROSOFT_APP_CERTIFICATE AND MICROSOFT_APP_PRIVATEKEY.
Those has been generated from the [Bot registration's credential step]({{< ref "bot-registration#credential-management" >}})

## Configuration

Environment variables or `.env`:

* `PORT`: Port to listen to (default: 3978)
* `MICROSOFT_APP_ID`: App registration application id
* `MICROSOFT_APP_PASSWORD`: Application password
* `MICROSOFT_APP_CERTIFICATE`: Base64 representation of the PEM certificate
* `MICROSOFT_APP_PRIVATEKEY`: Base64 representation of PEM privatekey
* `MICROSOFT_APP_TENANT_ID`: Tenant ID
* `DATABASE_URL`: Database DSN in the form: postgresql://{USER}:{PASSWORD}@{HOST}/{DATABASE}
