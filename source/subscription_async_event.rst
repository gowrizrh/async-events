Subscribing to Asynchronous Events
===================================

This section covers an example of creating a subscription which will receive notifications via HTTPS.

Subscribe to an async event by sending a POST request to the ``rest/V1/async_event`` endpoint with the async event
details in the request body.

Following our previous example of the ``sales.order.created`` async event, the request body would look like this:

.. code-block:: json

    {
        "asyncEvent": {
            "event_name": "sales.order.created",
            "recipient_url": "https://example.com/sales_order_created",
            "verification_token": "supersecret",
            "metadata": "http"
        }
    }

.. note::

    The usage of the ``verification_token`` field depends on the notifier being used. In this case, the HTTP notifier will use the verification token create
    a HMAC signature and include it as a ``x-magento-signature`` header in the notification request. Other notifiers may use
    this field differently or not at all.

Async Event Sinks
*****************

Async Events is protocol-agnostic, allowing subscriptions to be created using various transport mechanisms. When an
async event is published, the system looks for all subscriptions matching the event name and notifies each subscriber by
its preferred notifier.

By default, the system handles this using the ``metadata`` provided during subscription. (However this behaviour can be
changed). See advanced usage for more details.

AWS
---

The https://github.com/mage-os/mageos-async-events-aws package provides support for various AWS services as async event sinks.

* EventBridge
* SQS

GCP
---------------------

The https://github.com/mage-os/mageos-async-events-gcp package provides support for using Google Cloud Platform services as async event sinks.

* Pub/Sub
