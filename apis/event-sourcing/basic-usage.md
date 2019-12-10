---
description: Basic usage of the Event Sourcing API.
---

# Basic Usage

The most important concepts in Serialized are **Aggregates** and **Events**. Both these concepts are borrowed from the concepts Domain-Driven Design and it is important to understand the nuances of them to effectively use Serialized.

{% hint style="info" %}
We use the shorter term **Event** to refer to **\*\*the concept** Domain Event\*\* from Domain-Driven Design.
{% endhint %}

## Events

An **Event** in Serialized describes something important that have happened in your business that should be stored as a fact. Events are always part of a business process \(Aggregate\) so you will always refer to an Aggregate when working with Events in Serialized.

### Uniquely identified

Your events have a unique identifier that is of the format of UUID. You can supply this id in the API or you can omit id and let Serialized generate a random unique when the Event is stored.

### Event type

The `eventType` field of the Event describes the meaning of the Event. The Event type describes in business language what has happened.

{% hint style="warning" %}
Make sure to name your Event type in **past tense**. Events should describe something that happened in the past in order to make sense.
{% endhint %}

### Event data

In a system using Event Sourcing you store state purely in events. The `data` field in Serialized events provide the data that should be stored together with the Event.

{% hint style="info" %}
The `data` field is optional. Since events have a type that provide meaning to the application purely by their name, it is sometimes enough to store an event without additional data.
{% endhint %}

The format of the data field is a generic JSON object, so you can store any structured data within this field.

#### Encrypted data

Events also provides a special String-type field `encryptedData` for storing data that should be client-side encrypted before stored.

### Immutable

Since Events describe facts that have happened they are immutable. You can therefore not use the API to change an event that has been saved. To modify the current state you instead store another Event that is a reversal/compensation of the Event that you want to change and add handling of that event to downstream services such as Projections and Reactions.

{% hint style="success" %}
Serialized supports deletion of Events by deleting the Aggregate that contains the Event. In the case you accidentally stored secret data or have to clean your data by request from a customer we provide the support to do this.
{% endhint %}

## Event Example: Hotel Room Reservation

Let's say we're developing a Hotel Management System that handles reservations of hotel rooms and see how a reservation could be implemented as a Serialized Event:

> A **hotel room** for **Room 213** was **reserved** by **John Doe** at **2019-03-12** using **reservation number ABC-123**.

```javascript
{
  "events": [
    {
      "eventId": "f2c8bfc1-c702-4f1a-b295-ef113ed7c8be",
      "eventType": "HotelRoomReserved",
      "data": {
        "reservationNumber" : "ABC-123",
        "roomNumber": "213",
        "guests": ["John Doe"],
        "reservationDate" : "2019-03-12"
      }
    }
  ]
}
```

## Aggregates

Aggregates are ordered sequences of batches of Events.

It is your system and application code that will implement the business rules of the Aggregate and Serialized will take care of storing all Events for the Aggregate in an efficient manner and providing the APIs you need to manage the lifecycle of the Aggregate.

## Aggregate Example: Hotel Room Reservation

To connect the example of the `HotelRoomReserved` Event that we used in the previous example we can view the process of a **HotelRoomReservation** as our Aggregate. The reservation would contain multiple Events that refer to this specific reservation and business rules that only makes it possible for Events to occur in a certain order.

#### Confirming the Reservation

Pseudo-code for how your application could implement the use-case of confirming a reservation in a consistent manner looks like this:

1. Load the Aggregate events and current version.
2. Materialize a current state for the aggregate from the Events. In this case create a data structure with the availability of rooms for certain dates.
3. Validate that the room is still available for the date of the reservation.
4. Store a new `ReservationConfirmed` Event providing the version number loaded in step 1 to avoid any inconsistency or double-booking.

```javascript
{
  "events": [
    {
      "eventId": "1f112f10-ac94-416c-9b0a-5a5d521ceb66",
      "eventType": "ReservationConfirmed",
      "data": {
        "reservationNumber" : "ABC-123",
        "confirmationDate" : "2019-03-13"
      }
    }
  ],
  "expectedVersion" : 46
}
```

