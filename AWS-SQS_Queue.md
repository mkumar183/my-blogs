#### SQL Queue


[tutorial](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-getting-started.html)

#### Step 1: Create a queue
The first and most common Amazon SQS task is creating queues. This procedure shows how to create and configure a FIFO queue.

Open the Amazon SQS console at https://console.aws.amazon.com/sqs/.

Choose Create queue.

On the Create queue page, ensure that you set the correct region.

The Standard queue type is selected by default. Choose FIFO.

You can't change the queue type after you create a queue.

Enter a Name for your queue. The name of a FIFO queue must end with the .fifo suffix.

To create your queue with the default parameters, scroll to the bottom and choose Create Queue. Amazon SQS creates the queue and displays the queue's Details page.

Amazon SQS propagates information about the new queue across the system. Because Amazon SQS is a distributed system, you may experience a slight delay before the queue is displayed on the Queues page.

#### Step 2: Send a message
After you create your queue, you can send a message to it.

From the left navigation pane, choose Queues. From the queue list, select the queue that you created.

From Actions, choose Send and receive messages.


The console displays the Send and receive messages page.

Enter text in the Message body

Enter a Message group id for the queue. For more information, see FIFO delivery logic.

(Optional) Enter a Message deduplication id. If you enable content-based deduplication, the message deduplication ID is not required. For more information, see FIFO delivery logic.

Choose Send message.

Your message is sent and the console displays a success message. Choose View details to display information about the sent message.

https://sqs.eu-west-2.amazonaws.com/?Action=CreateQueue&DefaultVisibilityTimeout=40&QueueName=MyQueue&Version=2012-11-05&AUTHPARAMS


[Example of Signing API Request](https://docs.aws.amazon.com/general/latest/gr/sigv4-signed-request-examples.html)
