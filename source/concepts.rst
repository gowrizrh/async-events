Concepts
========

Async Events enables an event-driven architecture within Magento. There are a
few concepts to learn to fully grasp the power of async events.


Async Event
-----------

An async event is the topic or the name of an event that happens within Magento.
An async event is declared in an ``async_events.xml`` file. This declaration
contains the name and the schema of the event.

.. code-block:: xml

    <async_event name="sales.order.created">
        <service class="Magento\Sales\Api\OrderRepositoryInterface" method="get"/>
    </async_event>

.. tip::

   The schema is defined by the return type of the service class and the method.

   This is elegant because it allows you to reuse the same service methods as a
   REST API, which means any changes to the service method like field updates, plugins
   or extension attributes are also available in async event payloads!

Subscription
------------

A subscription declares a subscriber's intention to receive notifications for a
single event. A single subscriber can subscribe to multiple events and a single
event can have multiple different subscribers.

A subscription is defined by:

* The async event name
* The subscription endpoint
* The notifier

One neat thing about async events are they are protocol independent! This means
they are not bound to just HTTP destinations.

One can write a notifier to send events to even unix sockets or file
descriptors. However the most common alternate transports are

* AMQP
* MQTT
* Kafka

Notifier
--------

A notifier is responsible for delivering the event and the implementation details
are its own. For example a HTTP notifier might simply use ``curl_exec`` or a
requests library like ``Guzzle`` to deliver the event to the destination.

A slack notifier could use Slack's SDK to send messages to channels. It can
implement advanced features like threading and replies.