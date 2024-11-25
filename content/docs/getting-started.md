---
title: Getting Started
weight: 10
prev: /docs
#next: /docs/core
draft: false
---

All components are OCI images, sharing nothing except a PostgreSQL database, keeping no states, making them suitable for simple Kubenetes hosting (helm to come).

Service is composed of 2 core components:

* **bf-directline-endpoint**: the component in charge of interacting with the users on MS Teams through the bot framework

* **activity-api**: the component allowing you to publish, edit or delete messages on MS Teams

First of all you'll need to setup a bot for it.