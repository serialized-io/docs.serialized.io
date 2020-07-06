# Request signatures

All outgoing HTTP requests has the `User-Agent` header set to `Serialized/1.0` and includes a Serialized specific signature header that can be used to verify the request’s authenticity.

The header is named `Serialized-Request-Signature` and contains a [HMAC](https://en.wikipedia.org/wiki/HMAC) calculated using the HmacSHA256 algorithm, specified in RFC 2104 and FIPS PUB 180-2.

### Request types

The different request types are signed with different default keys. See the definition documentation for each type for details on how to provide you own custom signing secret.

| Request type | Default signing key |
| :--- | :--- |
| Reaction | Reaction definition name |
| Projection | Projection definition name |

### Example

{% tabs %}
{% tab title="Java/Jersey/Dropwizard" %}
```java
    import org.apache.commons.codec.digest.*;
    import javax.ws.rs.*;

    @POST
    @Path("notifications")
    public Response performNotification(@Context HttpHeaders headers, String body) {
      String signingKey = "notify-on-order-shipped";
      String receivedSignature = headers.getHeaderString("Serialized-Request-Signature");
      String calculatedSignature = new HmacUtils(HMAC_SHA_256, signingKey).hmacHex(body);

      if (!calculatedSignature.equals(receivedSignature)) {
        throw new WebApplicationException(BAD_REQUEST);
      }

      ...
    }
```
{% endtab %}
{% endtabs %}

