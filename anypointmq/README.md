# Anypoint MQ Bindings

This document defines how to describe MuleSoft's Anypoint MQ-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.1.0`.


<a name="server"></a>

## Server Binding Object

This object contains information about the server representation in Anypoint MQ.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="serverBindingObjectClientId"></a>`clientId` | string | The client identifier.
<a name="serverBindingObjectClientSecret"></a>`clientSecret` | string | The client secret.
<a name="serverBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Example

```yaml
servers:
  production:
    bindings:
      anypointmq:
        clientId: {clientId}
        clientSecret: {clientSecret}
        bindingVersion: 0.1.0
    variables:
      clientId:
        description: This value is represents the client application created in Anypoint MQ
      clientSecret:
        description: This value is represents the corresponding client secret in Anypoint MQ
```

<a name="channel"></a>

## Channel Binding Object

This object contains information about the channel representation in Anypoint MQ.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="channelBindingObjectIs"></a>`is` | string | Defines what type of channel is it. Can be either `queue` or `exchange` (default).
<a name="channelBindingObjectExchange"></a>`exchange` | Map[string, any] | When `is`=`exchange`, this object defines the queue properties.
<a name="channelBindingObjectExchangeName"></a>`exchange.name` | string | The name of the exchange. Must be unique and can only use alphanumeric, hyphens, or periods.
<a name="channelBindingObjectExchangeEncrypted"></a>`exchange.encrypted` | boolean | Whether the exchange should be encrypted.
<a name="channelBindingObjectExchangeBoundedQueues"></a>`exchange.boundedQueues` | [string] | Whether the exchange should be encrypted.
<a name="channelBindingObjectQueue"></a>`queue` | Map[string, any] | When `is`=`queue`, this object defines the queue properties.
<a name="channelBindingObjectQueueName"></a>`queue.name` | string | The name of the queue. Must be unique and can only use alphanumeric, hyphens, or periods.
<a name="channelBindingObjectQueueTtl"></a>`queue.ttl` | integer | Specifies how long unprocessed messages persist before being deleted (in minutes).
<a name="channelBindingObjectQueueAckTimeout"></a>`queue.ackTimeout` | integer | Specifies how long a message waits before being put back into the queue in the event of server failure when the message is not acknowledged (in milliseconds).
<a name="channelBindingObjectQueueDefaultDeliveryDelay"></a>`queue.defaultDeliveryDelay` | integer | Specifies how long to delay delivery for messages sent to the queue (in milliseconds).
<a name="channelBindingObjectQueueEncrypted"></a>`queue.encrypted` | boolean | Whether the queue should be encrypted.
<a name="channelBindingObjectQueueDeadLetter"></a>`queue.dlq` | string | The name of an associated dead letter queue, if required.
<a name="channelBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Example

```yaml
channels:
  user/signedup:
    bindings:
      anypointmq:
        is: exchange
        queue:
          name: user-registration-queue
          ttl: 14400
          ackTimeout: 120000
          defaultDeliveryDelay: 1000
          encrypted: true
          dlq: my-dead-letter-queue-name
        exchange:
          name: user-registration-exchange
          encrypted: true
          boundedQueues: ['user-registration-queue']
        bindingVersion: 0.1.0
```


<a name="operation"></a>

## Operation Binding Object

This object contains information about the operation representation in Anypoint MQ.

##### Fixed Fields

Field Name | Type | Applies To | Description
---|:---:|:---:|---
<a name="operationBindingObjectAcknowledgementMode"></a>`acknowledgementMode` | string | Subscribe | Acknowledgment mode to use for the messages retrieved from this subscriber. Can only be 'MANUAL' or 'NONE'.
<a name="operationBindingObjectPollTime"></a>`pollTime` | integer | Subscribe | How much time in milliseconds to wait if the requested messages are not ready to be consumed.
<a name="operationBindingObjectAcknowledgementTimeout"></a>`acknowledgementTimeout` | integer | Subscribe | Duration that a message is held by a broker waiting for an Acknowledgment (ack) or Not Acknowledgment (nack). After that duration expires, the message is again available to any subscriber.
<a name="operationBindingObjectMessageId"></a>`messageId` | string | Publish | ID of the message to publish.
<a name="operationBindingObjectSendContentType"></a>`sendContentType` | string | Publish | Indicates whether the content type of the Mule message should be attached or not.
<a name="operationBindingObjectOutputMimeType"></a>`outputMimeType` | string | Publish, Subscribe | MIME type of this operation’s output.
<a name="operationBindingObjectOutputEncoding"></a>`outputEncoding` | string | Publish, Subscribe | The encoding of this operation’s output.
<a name="operationBindingObjectReconnectionStrategy"></a>`reconnectionStrategy` | Map[string, any] | Publish, Subscribe | An object for the retry strategy
<a name="operationBindingObjectReconnectionStrategyType"></a>`reconnectionStrategy.type` | string | Publish, Subscribe | A retry strategy in case of connectivity errors. Its value MUST be either RECONNECT or RECON_FOREVER
<a name="operationBindingObjectReconnectionStrategyFrequency"></a>`reconnectionStrategy.frequency` | integer | Publish, Subscribe | How often in milliseconds to reconnect.
<a name="operationBindingObjectReconnectionStrategyCount"></a>`reconnectionStrategy.count` | integer | Publish, Subscribe | How many reconnection attempts to make.
<a name="operationBindingObjectReconnectionBlocking"></a>`reconnectionStrategy.blocking` | boolean | Publish, Subscribe | If false, the reconnection strategy runs in a separate, non-blocking thread.
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | Publish, Subscribe | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Example

```yaml
channels:
  user/signedup:
    subscribe:
      bindings:
        anypointmq:
          acknowledgementMode: NONE
          pollTime: 60000
          acknowledgementTimeout: 120000
          outputMimeType: application/json
          outputEncoding: UTF-8
          reconnectionStrategy:
            type: RECONNECT
            frequency: 10000
            count: 10
            blocking: true
          bindingVersion: 0.1.0
```


<a name="message"></a>

## Message Binding Object

This object contains information about the message representation in Anypoint MQ.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="messageBindingObjectMessageType"></a>`messageType` | string | Application-specific message type. Its value MUST be either TEXT, JSON or CSV.
<a name="messageBindingObjectDefaultDeliveryDelay"></a>`defaultDeliveryDelay` | integer | Specifies how long to delay delivery for messages sent to the queue (in milliseconds).
<a name="messageBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

```yaml
channels:
  user/signup:
    publish:
      message:
        bindings:
          anypointmq:
            messageType: JSON
            defaultDeliveryDelay: 1000
            bindingVersion: 0.1.0
```
