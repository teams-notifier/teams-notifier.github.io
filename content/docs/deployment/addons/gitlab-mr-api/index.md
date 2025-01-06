---
linkTitle: gitlab-mr-api
title: gitlab-mr-api
prev: /docs/deployment/addons/
next: /docs/user-manual/
---

**Teams Notifier's gitlab-mr-api component**

|   |   |
|---|---|
| **homepage** | https://github.com/teams-notifier/gitlab-mr-api |
| **registry** | https://github.com/teams-notifier/gitlab-mr-api/pkgs/container/gitlab-mr-api |
| **image** | `ghcr.io/teams-notifier/gitlab-mr-api:latest` |

## Intro

This component enables real-time tracking of project merge requests. When a merge request is created, a message is posted and automatically updated as the MR status changes. The message is automatically deleted when the merge request is either closed or merged. Deleted merge request's messages won't be removed because Gitlab doesn't call webhook on deletion.

Here's an example of a merge request message:
{{< img src="merge-request-card-example.png" style="margin-left: 0rem">}}

## Config

Environment variables or `.env`:

* `PORT`: port to listen to (def: `3980`)
* `ACTIVITY_API`: `activity-api` base URL (ex: `https://activity-api:3981/`)
* `DATABASE_URL`: Database DSN in the form: `postgresql://{USER}:{PASSWORD}@{HOST}/{DATABASE}?search_path=gitlab_mr_api`
* `VALID_X_GITLAB_TOKEN`: comma separated list of Gitlab's Secret token (sent as `X-Gitlab-Token` header), UUIDv4 generated token is recommended.

### Notes on `db/schema.sql`

The dump uses a dedicated schema but expects `pgcrypto` in the *public* schema for `uuid_generate_v7`.
If you plan to deploy the dump in another schema or if the extension has been deployed somewhere else, adapt the function definition and the dump accordingly.

To connect directly to the dedicated schema, you can use *search_path* argument of the DSN `?search_path=gitlab_mr_api`.

## Webhook config

You'll need:
- one or more conversation tokens
- one of the `VALID_X_GITLAB_TOKEN` you generated

On your GitLab repository, follow these steps to add a new webhook:

1. Go to **Settings**.
2. Select **Webhooks**.
3. Click **Add new webhook**.
4. Fill out the form with the following details:

   * **URL**: https://hostname-of-this-api.example.org/api/v1/gitlab-webhook \
      {{< callout type="info" >}}
      Since v0.12.0, you can specify a filter to publish notifications only if certain users are involved as the opener, reviewers, or assignees. Use query parameters `filter_on_participant_ids` with a comma-separated list of user IDs (integers).
      {{< /callout >}}

   * **Add custom header**:
     * name: `X-Conversation-Token`
     * value: comma separated list of conversation tokens you want the MR messages sent to
   * **Secret token**: one of the `VALID_X_GITLAB_TOKEN` you generated
   * **Trigger**, select:
     * Merge request events
     * Pipeline events
     * Emoji events (planned)

You can then save or test.
Note that for the test to work you need an existing MR in the project.

In case of failure, now or afterward, you can Edit the webhook and you'll see all the previous calls, error code and messages, including sent payload and a way to resend the request.

## Webhook declaration helper

{{% details title="Environment variables" %}}

To be placed in a `.env` file in the script's folder or `export`'ed

```bash
# Your Gitlab personal access token with Admin or Maintainer right
# Used to declare the webhook itself
GITLAB_TOKEN=

# your gitlab url (opt. if gitlab.com)
#GITLAB_URL=

# coma separated list of conversation tokens
CONVERSATION_TOKEN=

# Gitlab secret matching one of the gitlab-mr-api's VALID_X_GITLAB_TOKEN
# Mechanism used by Gitlab to validate received payloads
GITLAB_SECRET=

# Full url of the accessible webhook https://gl-webhook.example.org/api/v1/gitlab-webhook
HOOK_URL=
```
{{% /details %}}

{{% details title="`setup-webhook.sh`" %}}

To find the project's id, go to the project's homepage, at the rightmost top, click the vertical ellipsis and click *Copy project ID*. \
Tested on Gitlab 17.6.

```bash
#!/bin/bash

# Takes a project id as first arg
# Optional conversation token override as second
set -euo pipefail

[ -e .env ] && . .env

[ -z "${1:-}" ] && echo "please provide a project id as first arg" && exit 1

PROJECT_ID=${1}
CONVERSATION_TOKEN=${2:-$CONVERSATION_TOKEN}

GITLAB_URL=${GITLAB_URL:-gitlab.com}

HOOK_COUNT=$(curl -s --header "PRIVATE-TOKEN: $GITLAB_TOKEN" \
    https://${GITLAB_URL}/api/v4/projects/$PROJECT_ID/hooks/ \
    | jq 'map(select(.url | test("'${HOOK_URL}'")?)) | length')

[ "$HOOK_COUNT" -gt 0 ] && echo "already a hook declared" && exit 1

curl \
    -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
    -H 'Content-Type: application/json' \
    https://${GITLAB_URL}/api/v4/projects/$PROJECT_ID/hooks \
    -X POST -s -w $'\n' \
    --data-binary @- << EOF | jq .
{
"name": "gitlab-mr-api",
"url": "$HOOK_URL",
"token": "$GITLAB_SECRET",
"custom_headers": [{"key":"X-Conversation-Token", "value":"$CONVERSATION_TOKEN"}],
"merge_requests_events": true,
"pipeline_events": true,
"emoji_events": true,
"push_events": false,
"tag_push_events": false,
"repository_update_events": false,
"issues_events": false,
"confidential_issues_events": false,
"note_events": false,
"confidential_note_events": false,
"wiki_page_events": false,
"deployment_events": false,
"feature_flag_events": false,
"job_events": false,
"releases_events": false,
"resource_access_token_events": false
}
EOF
```
{{% /details %}}

