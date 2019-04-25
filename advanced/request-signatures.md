# Request signatures

All outgoing HTTP requests has the `User-Agent` header set to `Serialized/1.0` and includes a Serialized specific signature header that can be used to verify the request’s authenticity.

The header is named `Serialized-Request-Signature` and contains a [HMAC](https://en.wikipedia.org/wiki/HMAC) calculated using the HmacSHA256 algorithm, specified in RFC 2104 and FIPS PUB 180-2.

### Different request types

Different requests have different signatures, that you can use to verify the outgoing request from Serialized to your backend. 

| Request type | Signed data |
| :--- | :--- |
| Reaction | Reaction definition name |

### Example

{% tabs %}
{% tab title="Java/Jersey/Dropwizard" %}
```java
    import org.apache.commons.codec.digest.*;
    import javax.ws.rs.*;

    @POST
    @Path("notifications")
    public Response performNotification(@Context HttpHeaders headers, String body) {
      String expectedReactionName = "notify-on-order-shipped";
      String receivedSignature = headers.getHeaderString("Serialized-Request-Signature");
      String calculatedSignature = new HmacUtils(HMAC_SHA_256, expectedReactionName).hmacHex(body);

      if (!calculatedSignature.equals(receivedSignature)) {
        throw new WebApplicationException(BAD_REQUEST);
      }

      ...
    }
```
{% endtab %}
{% endtabs %}

