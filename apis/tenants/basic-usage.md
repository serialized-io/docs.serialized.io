# Basic Usage

When working with a multi-tenant enabled Serialized project, you need to add an additional HTTP header to each and every API call.

The parameter is called `Serialized-Tenant-Id` and is expected to contain the UUID of the tenant for which the API call is made.



