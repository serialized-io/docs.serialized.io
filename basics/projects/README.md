---
description: >-
  Projects are the container of your data. All projects are isolated from each
  other in the storage level.
---

# Projects

## Using Projects

Projects are the main entry point for all Serialized APIs. Your API keys are connected to a single project and your data is stored in a single project.

{% hint style="info" %}
You can create multiple projects for different use cases.
{% endhint %}

## Use cases for multiple projects

There are many scenarios where using multiple projects can be useful.

### Different environments

You can use different projects for separating data in different development environments \(development, test, production\).

### Different teams

You can provide teams with their separate projects where the data for their services can live in an isolated environment from each other.

### Different internal projects

You may have different internal development projects that should not be dependent on each other or interfere with each other, but you may still want to keep them under the same **Account** to collect your billing in an aggregated way.

### Multi-tenancy

For business-to-business scenarios it is common to develop software that is multi-tenant. This means that the software is shared between the customers but their data is separated. This is provided out-of-the box by Serialized by enabling the multi-tenancy feature for your project. Multi-tenancy is an extremely powerful feature that can potentially save a lot of maintenance time and resources.

{% hint style="info" %}
Multi-tenancy is a feature that is only available on our paid plans.
{% endhint %}

