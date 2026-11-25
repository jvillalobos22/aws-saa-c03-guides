## I. Introduction

1. PubSub model, useful for broadcasting messages.
2. The event producer only sends messages to one SNS topic.
3. Each subscriber to the topic will get all the messages.
4. Topic names are limited to 256 characters. Alphanumeric characters plus hyphens (-) and underscores (_) are allowed. Topic names must be unique within an AWS account.
5. Up to 10 million subscriptions per topic with a topic limit of 100K.
6. With the exception of SMS (140 bytes), the messages can be up to 256 bytes.
7. **Subscribers can be**
    1.  [[SQS]]
    2. **HTTP/HTTPS**
    3. **Lambda**
    4. **Email/SMS**
    5. **Mobile push notifications**
    6. **Kinesis**
8. Many internal apps use SNS
    1. CloudWatch
    2. ASG
    3. S3 (for bucket events)
    4. CloudFormation
9. Topic publish (using the SDK)
    1. Create a topic
    2. Create a subscription
    3. Publish to the topic
10. Direct publish
    1. Create a platform application
    2. Create a platform endpoint
    3. Publish to the platform endpoint
11. Security
    1. In-flight
    2. At-rest
    3. Client-side
    4. IAM for regulating access
    5. Policies for regulating like S3 are also available

## II. SNS + SQS (Fan out)

1. It's a common pattern to combine SNS and SQS.
2. SNS publishes a message once which is captured by multiple SQS listening subscribers.
![[Pasted image 20250514145024.png]]

3. Fully decoupled and no data loss.
4. **Make sure your SQS access policy allows for SNS to write.**
5. **SNS cannot send messages to SQS FIFO queues.**

## III. Delivery Retries

All messages sent to SNS are processed and delivered immediately. If a message cannot be delivered on the first attempt, **SNS implements a 4-phase retry policy**.

1. Retry with no delay in between attempts
2. Retry with some minimum delay
3. Retry with backoff
4. Retry with some maximum delay

## IV. Security

1. Topics can be encrypted server-side.
2. Supports VPC endpoints via AWS PrivateLink. You can use it to privately publish messages to SNS topics.
3. With access control policies, you can manage which topics are publishable and who can publish on them.
4. **You can enable X-Ray for messages passing through SNS.**

## V. SNS Tips

1. SNS provides durable storage of all messages that it receives.
2. **SNS allows you to set a TTL value for each message. When the TTL expires for a given message that wasn't delivered and read by an end-user, the message is deleted.**
3. **SNS cannot forward to FIFO SQS, only regular SQS.**
4. Supports delivery of message attributes (metadata around the message) that can help apply filter policies to topic subscriptions.
5. By default, messages are encoded in JSON.
6. If a lambda is subscribed to a SNS topic, it will invoke the function with the message payload as its argument.
7. SNS Mobile Push lets you use Simple Notification Service (SNS) to deliver push notifications to Apple, Google, Fire OS, and Windows devices.
8. You can use Amazon SNS to deliver SMS (text) messages to 200+ countries, and you do not require recipients to explicitly opt in as before. You must obtain prior permission from recipients to send SMS messages to their phone numbers, where required by local law and regulations.
9. While sending SMS, you can tag them as transactional (PIN, OTP etc) or promotional (ads). Transactional makes sure that the message is routed with the highest delivery reliability.
10. **SES is a cloud-based email sending service designed to send notification and transactional emails. Use SNS for alert emails.**
11. Topic names should typically be available for reuse approximately 30-60 seconds after the previous topic with the same name has been deleted.
12. SNS supports Email and Email-JSON. “Email-JSON” sends notifications as a JSON object, and is meant for applications to programmatically process emails. The ”Email” transport is meant for end-users/consumers and notifications are regular, text-based messages which are easily readable.
13. As part of the subscription registration, Amazon SNS will ensure that notifications are only sent to valid, registered subscribers/end-points e.g. by sending a confirmation email to the user. These explicit "opt-in" methods are not always required e.g. when the subscriber is a SQS queue in the same account as the original SNS.
14. SNS provides durable storage of all messages that it receives. Up on receiving a publish request, SNS stores multiple copies (to disk) of the message across multiple Availability Zones before acknowledging receipt of the request to the sender.
15. **SNS will attempt to deliver messages from the publisher in the order they were published into the topic. However, network issues could potentially result in out-of-order messages at the subscriber end.**
16. **Once a message has been successfully published to a topic, it cannot be recalled.**
17. Want history of all API calls and the responses given OUT by SNS for audit purposes? Use SNS logging to S3 bucket!