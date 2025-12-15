Publishing an Async Event
==========================

The ``Magento\Framework\MessageQueue\PublisherInterface`` can be used to publish async events by providing the async
event key and the event queue.


.. code-block:: php

   <?php

    use MageOS\AsyncEvents\Helper\QueueMetadataInterface;

    // ...

        public function sendOrder(Order $order): void
        {
            $arguments = ["id" => $order->getId()];

            $this->publisher->publish(
                QueueMetadataInterface::EVENT_QUEUE,
                [
                    "sales.order.created",
                    $this->json->serialize($arguments)
                ]
            );
        }
