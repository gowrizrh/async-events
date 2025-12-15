Defining an Async Event
=======================

This module does not ship with any predefined events; it focuses solely on providing the runtime.

For example, the `mageos-common-async-events` project defines a set of commonly used asynchronous events and implements
the logic that determines when an event is triggered and how duplicate events are handled.

When defining an asynchronous event, consider the following two aspects:

1. What is the event?
2. When does the event occur?

Naming Events
-----------------

It is conventional to name events using dot-separated words, ending with a verb; for example, ``sales.order.created``,
``inventory.stock.depleted``, or ``invoice.paid``.

The first step is to define the asynchronous event itself. For example, the following snippet declares an async event
named sales.order.created:

.. code-block:: xml

  <async_event name="sales.order.created">
  </async_event>

After the event semantics are established, the next step is to specify the data carried by each instance of the event.
This is done by defining how the event payload is resolved, as shown below:

.. code-block:: xml

    <async_event name="sales.order.created">
        <service class="Magento\Sales\Api\OrderRepositoryInterface" method="get"/>
    </async_event>

.. note::

  This configuration defines how the event payload is resolved, in a manner similar to ``webapi.xml``. The schema and
  structure of the payload are implicitly determined by the return type of the service method declared above.

You may notice that, in the example above, the ``OrderRepositoryInterface::get`` method requires an argument. How this
argument is provided to the async event runtime is explained in the next section.
