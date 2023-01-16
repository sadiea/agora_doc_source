## v4.0.1

v4.0.1 was released on January xx, 2023.

#### New features

**1. Media push**

This release adds the media push function, whereby hosts can convert the protocol of media streams to the RTMP protocol and push media streams to CDN. Audience members can subscribe to media streams by clicking the corresponding URL.

This release provides the `IRtmpConnection` and `IRtmpLocalUser` classes to connect the SDK and CDN, set audio and video encoding configurations, and push media streams to CDN. To get the connection state to CDN and the push state of media streams, you can register `IRtmpConnectionObserver` and `IRtmpLocalUserObserver` objects to listen for the related callbacks.

**2. Stream message**

To support sending and receiving messages in digital scenarios such as an avatar, this release adds the stream message function. A sender can call the `createDataStream` and `sendStreamMessage` methods in sequence to create a stream and send a stream message; a receiver can get data in the `onStreamMessage` callback when the reception is successful.

#### Improvements

**1. Custom background of custom video renderer**

To make video interactions more fun, users sometimes want to modify the background of a remote user to match their own preferences. To meet such requirements, this release adds the `alphaBuffer` parameter in the `ExternalVideoFrame` class. In a custom video renderer scenario, you can use this parameter to render the video background into various effects, such as transparent, solid color, picture, or video. Multiple users can render different video backgrounds locally for the same remote user without affecting each other.

<div class="alert note">To enable this function, contact <a href="mailto:support@agora.io">support@agora.io</a >.</div>

**2. Audio quality of experience**

This release adds the `qoe_quality` and `quality_changed_reason` parameters in the `RemoteAudioTrackStats` class to monitor the Quality of Experience (QoE) of the local user when receiving a remote audio stream and to provide the reason for poor QoE.

**3. Meeting scenarios**

To improve the user experience in meeting scenarios, this release adds `AUDIO_SCENARIO_MEETING(8)` in `AUDIO_SCENARIO_TYPE`.

**4. Domain restrictions for IoT SIM cards**

Some scenarios with high security requirements use targeted Internet of Things (IoT) SIM cards and restrict the network domains that IoT SIM cards can access. This release adds the `domainLimit` parameter in the `AgoraServiceConfiguration` class. You can enable domain restrictions and set the SDK to only access networks with the specified domain name, so that you can ensure that devices using a targeted IoT SIM card are interoperable with other devices.


<div class="alert note">To enable this function, contact <a href="mailto:support@agora.io">support@agora.io</a >.</div>