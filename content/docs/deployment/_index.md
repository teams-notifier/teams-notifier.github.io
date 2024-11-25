---
title: Deployment
weight: 1000
prev: /docs/getting-started
next: /docs/deployment/bot-registration
---

All components are OCI images, sharing nothing except a PostgreSQL database, keeping no states, making them suitable for simple Kubernetes hosting (helm coming soon).

Service is composed of 2 core components:

* **bf-directline-endpoint**: the component in charge of interacting with the users on MS Teams through the bot framework

* **activity-api**: the component allowing you to publish, edit or delete messages on MS Teams

First of all you'll need to setup a bot for it.