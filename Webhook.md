# Webhook

Consider the scenario that the payment platform wants to let you know about the result of the payment. How the platform should communicate with your application.

## Traditional Solutions

The main option against using Webhooks is using Polling.

### Short Polling

Traditionally, **Short Polling** was used. It means that you application would repeatedly asked for the result of the payment. **This kind of mechanism can be a bit of a resource hog.**

### Long Polling

**Long Polling** is a more patient sibling of short polling. Instead of constantly asking for the updates, the server request open for a while and one responds when there's new information. **This is better but not that much.**

## Using Webhooks

When using webhooks your application says to hit me at this URL whenever you had something to update me. In this case, the application has only the responsibility of handling update requests from the target platform. Also the responsibility of updating is now for the target platform. **No more wasted resources and constant nagging**.

> **Tip:** There are some other names for webhook such as reverse or push API

## Best Practices

### 1. Have a fallback polling mechanism

The applications should have a fallback polling mechanism in place to detect failed deliveries. It would happen that the target platform fails to deliver the update. 

## 2. Secure webhooks with secrets and tokens

You do not want just anyone calling the webhook endpoint. So, It is important to require authentication tokens to prevent abuse.

## 3. Make webhook idempotent

On the other hand, you should handle nothing bad happens if the webhook is delivered more than once.

## 4. Watch out for webhook overload

If your application get popular, is the infrastructure of your application handle a surge in webhook traffic?  You might need to use queues to decouple the receiving and processing of webhook events.