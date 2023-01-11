## v4.0.1.4

v4.1.0.4 was released on January xx, 2023.

#### New features

**Stream message**

To support sending and receiving messages in digital scenarios such as digital people, this release adds the stream message function. A sender can call the `createDataStream` and `sendStreamMessage` methods in sequence to create a stream and send a stream message; a receiver can get data in the `onStreamMessage` callback when the reception is successful.