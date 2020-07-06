# Partitioned feeding

It is possible to devide a feed into partitions on request to enable parallelized feed consumption. The partitioning will be done on the `aggregateId` field with a modulo operation.

The two query parameters controlling this behaviour is `partitionCount` and `partitionNumber`.

The `partitionCount`parameter specifies the total number of partitions and the `partitionNumber` parameter specified which partition to return in the response.

{% hint style="info" %}
Note that the `partitionNumber` is zero based, i.e. the first partition will be `0`.
{% endhint %}

The following query parameters would return a response containing only the first partition out of two.

```text
?partitionCount=2&partitionNumber=0
```

