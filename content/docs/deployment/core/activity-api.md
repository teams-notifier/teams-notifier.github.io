---
title: Activity API
weight: 40
next: /docs/deployment/addons/
---

*Teams Notifier's activity api component. Used to send/update/delete messages (activities) to MS Teams.*

|   |   |
|---|---|
| **homepage** | https://github.com/teams-notifier/activity-api |
| **registry** | https://github.com/teams-notifier/activity-api/pkgs/container/activity-api |
| **image** | `ghcr.io/teams-notifier/activity-api:latest` |


## Intro

A microservice component that manages messages (activities) in Microsoft Teams:
- Send new messages
- Update existing messages
- Delete messages

Shares it's database with `bf-directline-endpoint`.

## Authentication

Uses one of two methods:
1. Password-based: `MICROSOFT_APP_PASSWORD`
2. Certificate-based: Both `MICROSOFT_APP_CERTIFICATE` + `MICROSOFT_APP_PRIVATEKEY`

## Configuration

Environment variables (can be set via `.env` file):

### Server Settings
- `PORT`: Server port (default: 3980)

### Microsoft App Settings
- `MICROSOFT_APP_ID`: Application ID from app registration
- `MICROSOFT_APP_TENANT_ID`: Azure AD Tenant ID
- `MICROSOFT_APP_PASSWORD`: App secret/password
- `MICROSOFT_APP_CERTIFICATE`: PEM certificate (Base64 encoded)
- `MICROSOFT_APP_PRIVATEKEY`: PEM private key (Base64 encoded)

### Database Settings
- `DATABASE_URL`: PostgreSQL connection string
  Format: `postgresql://{USER}:{PASSWORD}@{HOST}/{DATABASE}`

## API Documentation

Interactive documentation available at:
- `/docs` - Swagger UI
- `/redoc` - ReDoc UI

### Send Messages

All POST routes require a `conversation_token` (obtained from MS Teams bot interaction) and returns a `message_id`:
```json
{
  "message_id": "uuid-v7"
}
```

#### Message options

1. **Text Message** `POST /api/v1/message/text`
```json
{
    "conversation_token": "conversation_token (uuid)",
    "text": "Your message content"
}
```

2. Simple Message `POST /api/v1/message/simple`
```json
{
    "conversation_token": "conversation_token (uuid)",
    "message": {
        "title": "Message title",
        "text": "Your message content",

        "style": "default", // Options: default, emphasis, good, attention, warning, accent
        "bleed": false, // Set the text container to bleed (fill the card width)

        "title_color": "default", // Options: dark, light, accent, good, warning, attention
        "title_style": "default", // Options: default, emphasis, good, attention, warning, accent
        "title_bleed": false, // Set the title container to bleed (fill the card width)

        "summary": "string"
    }
}
```

3. Simple Message `POST /api/v1/message/card`
```json
{
    "conversation_token": "conversation_token (uuid)",
    "card": {}, // Teams Adaptive Card object
    "summary": "Notification summary"
}
```

4. A generic one that can take any of the previous payload `POST /api/v1/message`
```json
{
    "conversation_token": "conversation_token (uuid)",
    // [... one of the previous payload...]
}
```

### Update a message

To update a message, send a `PATCH /api/v1/message` with one of the previous payload (not necessarily of the same kind), without the `conversation_token` but the provided `message_id`.

### Delete a message

Just send a `DELETE /api/v1/message` without the `conversation_token` but the provided `message_id`.
