<properties
 pageTitle="IoT Hub MQTT-Unterstützung | Microsoft Azure"
 description="Beschreibung der MQTT-Unterstützung auf IoT Hub-Ebene"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="07/19/2016"
 ms.author="dobett"/>

# IoT Hub MQTT-Unterstützung

IoT Hub ermöglicht Geräten mithilfe des Protokolls [MQTT v3.1.1][lnk-mqtt-org] das Kommunizieren mit den IoT Hub-Geräteendpunkten auf Port 8883. IoT Hub setzt voraus, dass die gesamte Gerätekommunikation mithilfe von TLS/SSL gesichert wird.

## Herstellen einer Verbindung mit einem IoT Hub

Ein Gerät kann sich entweder über die Bibliotheken in den [Microsoft Azure IoT SDKs][lnk-device-sdks] oder direkt über das Protokoll MQTT mit einem IoT Hub verbinden.

## Verwenden der SDKs für Geräteclients

[SDKs für Geräteclients][lnk-device-sdks] mit Unterstützung des Protokolls MQTT stehen für Java, Node.js, C und C# zur Verfügung. Die SDKs für Geräteclients verwenden die standardmäßige IoT Hub-Verbindungszeichenfolge zum Herstellen einer Verbindung mit einem IoT Hub. Um das MQTT-Protokoll verwenden zu können, muss der Clientprotokollparameter auf **MQTT** festgelegt werden. Standardmäßig verbinden sich die SDKs von Geräteclients mit einem IoT Hub, indem das **CleanSession**-Flag auf **0** festgelegt und **QoS-1** für den Nachrichtenaustausch mit dem IoT Hub verwendet wird.

Wenn ein Gerät mit einem IoT Hub verbunden ist, bieten die SDKs von Geräteclients Methoden, die dem Gerät das Senden von Nachrichten an und Empfangen von Nachrichten von einem IoT Hub ermöglichen.

Die folgende Tabelle enthält Links zu Codebeispielen für jede unterstützte Sprache und gibt die Parameter zum Herstellen einer Verbindung mit einem IoT Hub über das Protokoll MQTT an.

| Sprache | „Protocol“-Parameter |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | azure-iot-device-mqtt |
| [Java][lnk-sample-java] | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] | MQTT\_Protocol |
| [C#][lnk-sample-csharp] | TransportType.Mqtt |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

## Direktes Verwenden des Protokolls MQTT

Wenn ein Gerät die SDKs von Geräteclients nicht verwenden kann, lässt es sich dennoch mithilfe des Protokolls MQTT mit den öffentlichen Geräteendpunkten verbinden. Das Gerät muss im **CONNECT**-Paket die folgenden Werte verwenden:

- Verwenden Sie für das Feld **Client-ID** die **Geräte-ID**.
- Verwenden Sie `{iothubhostname}/{device_id}` für das Feld **Benutzername**, wobei {iothubhostname} der vollständige CNAME für den IoT Hub ist.

    Beispiel: Wenn der Name für den IoT Hub **contoso.azure devices.net** und der Name des Geräts **MyDevice01** lautet, sollte das vollständige Feld **Benutzername** den Namen `contoso.azure-devices.net/MyDevice01` enthalten.

- Verwenden Sie im Feld **Kennwort** ein SAS-Token. Das Format des SAS-Tokens ist das gleiche wie für die Protokolle HTTP und AMQP: <br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Weitere Informationen zum Generieren von SAS-Token finden Sie unter [Verwenden von IoT Hub-Sicherheitstoken][lnk-sas-tokens] im Abschnitt zu Geräten.
    
    Beim Testen können Sie auch das Tool [Device Explorer][lnk-device-explorer] nutzen, um schnell ein SAS-Token zu generieren, das Sie kopieren und in Ihren eigenen Code einfügen können:
    
    1. Klicken Sie in Device Explorer auf die Registerkarte **Management**.
    2. Klicken Sie auf **SAS Token** (oben rechts).
    3. Wählen Sie unter **SASTokenForm** Ihr Gerät in der Dropdownliste **DeviceID** aus. Legen Sie Ihre **TTL** fest.
    4. Klicken Sie auf **Generieren**, um Ihr Token zu erstellen.
    
    Das generierte SAS-Token sieht folgendermaßen aus: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

    Folgender Teil wird im Feld **Kennwort** verwendet, um über MQTT eine Verbindung herzustellen: `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

Für die MQTT-Pakete CONNECT und DISCONNECT löst IoT Hub ein Ereignis im Kanal **Vorgangsüberwachung** aus.

### Senden von Nachrichten an einen IoT Hub

Nachdem Sie erfolgreich eine Verbindung hergestellt haben, kann ein Gerät Nachrichten mithilfe von `devices/{device_id}/messages/events/`oder `devices/{device_id}/messages/events/{property_bag}` als **Themenname** an IoT Hub senden. Das `{property_bag}`-Element ermöglicht dem Gerät das Senden von Nachrichten mit zusätzlichen Eigenschaften in einem URL-codierten Format. Beispiel:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] Dies ist die gleiche Codierung wie bei Abfragezeichenfolgen im HTTP-Protokoll.

Die Geräteclientanwendung kann auch `devices/{device_id}/messages/events/{property_bag}` als **Will-Themennamen** verwenden, um festzulegen, dass *Will-Nachrichten* als Telemetrienachricht weitergeleitet werden.

### Empfangen von Nachrichten

Zum Empfangen von Nachrichten von IoT Hub muss für ein Gerät ein Abonnement unter Verwendung von `devices/{device_id}/messages/devicebound/#”` als **Themenfilter** eingerichtet werden. IoT Hub liefert Nachrichten mit dem **Themennamen** `devices/{device_id}/messages/devicebound/` oder `devices/{device_id}/messages/devicebound/{property_bag}`, wenn Nachrichteneigenschaften vorhanden sind. `{property_bag}` enthält URL-codierte Schlüssel-Wert-Paare von Nachrichteneigenschaften. Nur Anwendungseigenschaften und vom Benutzer festlegbare Systemeigenschaften (z.B. **messageId** oder **correlationId**) sind im Eigenschaftenbehälter enthalten. Systemeigenschaftennamen haben das Präfix **$**, Anwendungseigenschaften verwenden den ursprünglichen Eigenschaftennamen ohne Präfix.

## Nächste Schritte

Weitere Informationen zur MQTT-Unterstützung mit den IoT-Geräte-SDKs finden Sie unter [Hinweise zur MQTT-Unterstützung][lnk-mqtt-devguide] im Entwicklungsleitfaden für Azure IoT Hub.

Weitere Informationen zum Protokoll MQTT finden Sie in der [MQTT-Dokumentation][lnk-mqtt-docs].

Weitere Informationen zum Planen Ihrer IoT Hub-Bereitstellung finden Sie unter:

- [Unterstützte Geräte][lnk-devices]
- [Unterstützen zusätzlicher Protokolle][lnk-protocols]
- [Vergleich mit Event Hubs][lnk-compare]
- [Skalierung, hohe Verfügbarkeit und Notfallwiederherstellung][lnk-scaling]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Entwicklerhandbuch][lnk-devguide]
- [Erkunden der Geräteverwaltung mithilfe der Beispielbenutzeroberfläche][lnk-dmui]
- [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]
- [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-sas-tokens.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide.md#mqtt-support

[lnk-devices]: iot-hub-tested-configurations.md
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0720_2016-->