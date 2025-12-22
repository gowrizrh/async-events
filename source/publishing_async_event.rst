Publishing an Async Event
==========================

Parameters
**********

When an event is published, additional parameters may be required for runtime resolution.

Consider the sales order get REST API. The ``webapi.xml`` is defined as below

.. code-block:: xml

    <route url="/V1/orders/:id" method="GET">
        <service class="Magento\Sales\Api\OrderRepositoryInterface" method="get"/>
    </route>

Note that the ``:id`` path parameter maps to the ``$id`` argument of the ``Magento\Sales\Api\OrderRepositoryInterface::get``
method.

.. code-block:: php

    <?php

    interface OrderRepositoryInterface
    {

        /**
        * ...
        *
        * @param int $id The order ID.
        * @return \Magento\Sales\Api\Data\OrderInterface Order interface.
        */
        public function get($id);

This mapping would occur when the API is called with a request such as:

.. code-block:: shell

    curl https://store.example.com/rest/V1/orders/1


Async Events
------------

The same principle applies to asynchronous events. However, instead of relying on a caller supplying the required
parameters, they can be resolved at the time the event occurs, allowing the event to be published with the necessary
data to resolve it asynchronously (later).

This mechanism is what enables async events to use a push-based model in contrast to the pull-based model used by REST
APIs.

In the ``sales.order.created`` example, analogous to the sales order get API, an ``id`` argument is required.

.. code-block:: php
   :emphasize-lines: 4

    <?php

        $arguments = [
            "id" => $order->getId();
        ]


An async event can be published using the ``MageOS\AsyncEvents\Api\AsyncEventPublisherInterface``.

Provide the event name and the required arguments to the ``publish`` method. The store ID is optional. If not
provided, all subscribers to the event will be notified.

.. code-block:: php

   <?php

    use MageOS\AsyncEvents\Api\AsyncEventPublisherInterface;

    // ...

        public function publishOrder(Order $order): void
        {
            $arguments = [
                "id" => $order->getId();
            ]

            $this->publisher->publish(
                    "sales.order.created",
                    $arguments,
                    $storeId // optional
                ]
            );
        }

Creating a subscription for an async event is covered in the next section.