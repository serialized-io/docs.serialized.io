# Basic Usage

When working with a multi-tenant enabled Serialized project, you need to add an additional HTTP header to each and every API call.

The parameter is called `Serialized-Tenant-Id` and is expected to contain the UUID of the tenant for which the API call is made.

#### Exceptions

The tenant ID header must not be sent when calling the API for handling Reaction definitions \(`/reactions/definitions`\) or Projection definitions \(`/projections/definitions`\), as they are shared between all tenants.

The same goes, naturally, for the Tenant API itself, `/tenants`.



