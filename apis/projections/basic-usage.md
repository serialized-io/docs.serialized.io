---
description: This page describes the basic usage of the Projections API.
---

# Basic Usage

Your projection definition consists of a number of event handlers that are applied to your events in sequence, to modify the current state of a projection. Each event handler defines processing logic for a particular event type and includes a number of options for filtering/matching event data and producing different outputs. This is how you can easily create and adapt client-specific views based on your events.

Event handlers are defined in the `handlers` section of your projection definition. You can add any number of handlers to your projection definition, even though it’s likely that a few will suffice for most cases.

