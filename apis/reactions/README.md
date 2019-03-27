---
description: >-
  The Event Reaction API provides support for connecting your events to
  side-effect behavior such as sending e-mails, calling your backend or any
  other external service.
---

# Reactions API

Reactions are configured to react to a specific event and trigger an action whenever that event is processed by the event reactor. An example could be sending a confirmation email to a customer when an order has been successfully placed.

It is not possible for a Reaction to trigger on an event that has happened in the past. Reactions are always configured to listen to events from the moment when the Reaction is configured.
