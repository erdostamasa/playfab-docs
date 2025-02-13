---
title: Xbox XDK Prerequisites
description: Xbox XDK supplement for PlayFab Party quickstart
author: ScottMunroMS
ms.author: scmunro
ms.date: 11/5/2019
ms.topic: article
ms.service: playfab
keywords: playfab, multiplayer, networking
---

# Xbox XDK Prerequisites

## Xbox One exclusive resource application network manifest templates

In its app package manifest, the app should declare the <em>internetClientServer</em> and <em>privateNetworkClientServer</em> capabilities, because an application using Party requires connecting to and accepting connections from network resources, both over the Internet and the local network. The app should also declare the <em>microphone</em> device capability, as Party requires access to microphone devices to support voice chat. For more detail about these capability settings, see the platform documentation.

The following snippet shows the nodes that must exist under the Package/Capabilities node in order to enable connectivity and voice communication. An Xbox One exclusive resource application that uses PlayFab Party must ensure certain socket description and template content is included in its "network manifest" extension of the app package manifest. See the platform documentation for more detail on network package manifests.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/2010/manifest"
  xmlns:mx="http://schemas.microsoft.com/appx/2013/xbox/manifest"
  IgnorableNamespaces="mx">
  ...
  <Capabilities>
    <Capability Name="internetClientServer" />
    <Capability Name="privateNetworkClientServer" />
    <DeviceCapability Name="microphone" />
  </Capabilities>
  ...
  <Applications>
    <Application ...>
      <Extensions>
        <mx:Extension Category="windows.xbox.networking">
          <mx:XboxNetworkingManifest>
            <mx:SocketDescriptions>
              <mx:SocketDescription Name="PlayFabPartyUdpInitiatorToCloudService" SecureIpProtocol="Udp" BoundPort="0">
                <mx:AllowedUsages>
                  <mx:SecureDeviceSocketUsage Type="Initiate" />
                  <mx:SecureDeviceSocketUsage Type="SendInsecure" />
                  <mx:SecureDeviceSocketUsage Type="ReceiveInsecure" />
                </mx:AllowedUsages>
              </mx:SocketDescription>
              <mx:SocketDescription Name="PlayFabPartyUdpAcceptorOnCloudService" SecureIpProtocol="Udp" BoundPort="1-65535">
                <mx:AllowedUsages>
                  <mx:SecureDeviceSocketUsage Type="Accept" />
                  <mx:SecureDeviceSocketUsage Type="SendInsecure" />
                  <mx:SecureDeviceSocketUsage Type="ReceiveInsecure" />
                </mx:AllowedUsages>
              </mx:SocketDescription>
            </mx:SocketDescriptions>
            <mx:SecureDeviceAssociationTemplates>
              <mx:SecureDeviceAssociationTemplate Name="PlayFabPartyClientToCloudServiceUdp" InitiatorSocketDescription="PlayFabPartyUdpInitiatorToCloudService" AcceptorSocketDescription="PlayFabPartyUdpAcceptorOnCloudService" MultiplayerSessionRequirement="None">
                <mx:AllowedUsages>
                  <mx:SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
                  <mx:SecureDeviceAssociationUsage Type="AcceptOnOtherDevice" />
                </mx:AllowedUsages>
              </mx:SecureDeviceAssociationTemplate>
            </mx:SecureDeviceAssociationTemplates>
          </mx:XboxNetworkingManifest>
        </mx:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

## Next steps
- [Quickstart for PlayFab Party](quickstart.md)
- [Xbox Requirements](xbox-requirements.md)
- [Using MPSD](using-mpsd.md)
