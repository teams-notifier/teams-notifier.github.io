---
linkTitle: jira-api
title: jira-api
prev: /docs/deployment/addons/gitlab-mr-api
next: /docs/user-manual/
---

**Teams Notifier's jira-api component**

|   |   |
|---|---|
| **homepage** | https://github.com/teams-notifier/jira-api |
| **registry** | https://github.com/teams-notifier/jira-api/pkgs/container/jira-api |
| **image** | `ghcr.io/teams-notifier/jira-api:latest` |

## Intro

This component enables notification from Jira Automation to MS Teams card.

Here's an example of a Jira issue messages from default to fully expanded:
{{< img src="jira-card-example-original.png" style="margin-left: 0rem">}}
{{< img src="jira-card-example-level1.png" style="margin-left: 0rem">}}
{{< img src="jira-card-example-full.png" style="margin-left: 0rem">}}

## Config

Environment variables or `.env`:

* `PORT`: port to listen to (def: `8080`)
* `ACTIVITY_API`: `activity-api` base URL (ex: `https://activity-api:3981/`)
* `VALID_X_SHARED_SECRET_TOKEN`: comma separated list of Gitlab's Secret token (sent as `X-Shared-Secret-Token` header). A UUIDv4 generated token is recommended.

## Jira Automation usage

You'll need:
- one or more conversation tokens
- one of the `VALID_X_SHARED_SECRET_TOKEN` you generated

On your Jira Automation, in a "Send web request" action:

* **Web request URL**: https://hostname-of-this-api.example.org/api/v1/issue
* **HTTP method**: POST
* **Web request body**: Either *Issue data (Jira format)* or *Issue data (Automation format)*
* **Headers (optional)**: add two headers
  * **X-Shared-Secret-Token**: One of the `VALID_X_SHARED_SECRET_TOKEN` you generated
  * **X-Conversation-Token**: comma separated list of conversation tokens you want the Issue sent to
