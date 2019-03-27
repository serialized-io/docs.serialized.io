# Sequence numbers

When an event batch is persisted in the store it is assigned a sequence number. Each feed has its own ever increasing sequence number. This number is what the feed subscribers use to keep track of how much of the feed has been consumed. The number is guaranteed to be a unique, ever increasing, positive integer.



