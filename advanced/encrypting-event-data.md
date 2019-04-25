# Encrypting Event data

If you need to store personal or sensitive data in your events you can populate the field`encryptedData`, which is reserved for this kind of client-side encrypted data. This field will not be processed in any way and can contain anything packed into a `string`. 

Preferably you encrypt a JSON payload using symmetric encryption. Remember to keep the secret key safe! As you are not sharing the key with us, there’s no way we can help you recover it or access your encrypted data if you loose it. Note that the field is limited to max `65535` bytes.

Example event payload utilizing the `encryptedData` field:

```javascript
{
  "aggregateId": "723ecfce-14e9-4889-98d5-a3d0ad54912f",
  "events": [
    {
      "eventId": "127b80b5-4a05-4774-b870-1c9a2e2a27a3",
      "eventType": "OrderPlacedEvent",
      "encryptedData": "9cb2948ea01673aea153ef5a9618d4debf2c90385ec958470fc13...",
      "data": {
        "customerId": "some-test-id-1",
        "orderAmount": 12345
      }
    }
  ]
}
```

