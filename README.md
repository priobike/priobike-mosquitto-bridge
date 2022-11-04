# PrioBike Mosquitto Bridge

A custom Mosquitto broker that enforces the retain flag on all incoming messages.

Current version: `2.0.14`

## Why?

Sometimes it is not possible to control how MQTT messages are published. In our case, we have an external provider that manages the MQTT broker and publishes messages without the retain flag. This means that the messages are only delivered to clients that are connected at the time of publishing. This is not ideal for us, as we want to be able to receive all messages, even if we are not connected at the time of publishing.

Thus, we created a custom Mosquitto broker that enforces the retain flag on all incoming messages. This way, we can always receive all messages, even if we are not connected at the time of publishing. Hence, this broker is called *Bridge* because it is meant to be used as a bridge between the external provider and our own clients.

This work is based on [this answer](https://forum.cedalo.com/t/setup-mosquitto-to-treat-all-messages-as-retained/296/2) and [this code](https://github.com/eclipse/mosquitto/blob/1382fded00fa0f77d1eb04a3e0279ee3a0500681/plugins/examples/force-retain/mosquitto_force_retain.c) by Roger Light.

## Quickstart

Build the Docker image:

```
docker build -t priobike-mosquitto-bridge .
```

Run the Docker image:

```
docker run -it -p 1883:1883 mosquitto-bridge mosquitto -c /mosquitto-no-auth.conf
```

Publish a message:

```
mosquitto_pub -h localhost -t test -m "Hello World"
```

**After publishing,** connect to the broker and subscribe to the topic:

```
mosquitto_sub -h localhost -t test
```

Because this broker enforces the retain flag, you should see the message you published earlier 🎉.
