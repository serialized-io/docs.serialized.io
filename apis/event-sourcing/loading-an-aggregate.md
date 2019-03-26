# Loading an aggregate

Before appending events to your aggregate you typically load \(hydrate\) it by loading all previous events for that particular `aggregateId` and fast-forward it into its current state. To limit the size of the reponse the optional query parameters `since` and `limit` can be used. Note that the parameters represents _changes_ to the aggregate which does not necessarily correspond to the number of events that has been appended to it. The default `limit` is set to `1000`.

