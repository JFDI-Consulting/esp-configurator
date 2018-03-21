# esp-configurator
A central configuration architecture for Espressif ESP8266/ESP32 based devices

IoT device security is really important. Sure, your temperature data might not be interesting to hackers. But stopping them turning your sensor network into a highly capable botnet is really important.

We can secure communications on ESP8266 and ESP32 devices with encryption, using TLS. It takes a lot of memory, and we generally need that memory for other things - like doing the actual work the device is built for. It's tempting not to use encryption, or to avoid server verification. We used to have to verify server identity by SHA1 fingerprint, but since the fingerprint changes every time the server certificate changes, that's a non-starter for low-maintenance devices. So in recent ESP8266 SDK updates, we've been given Root CA Certificate verification, which is much better.

Why verify the server? We need to ensure the server we're talking to is actually the server we think it is. If we don't, we're vulnerable to man-in-the-middle attacks. One of the biggest threats to IoT sensor networks is the devastating combination of no server verification and OTA firmware upgradability.

OK, that's security. So what's this about central configuration?

It's a pain to commission large numbers of devices. Half the point of these low-cost devices is that it becomes affordable to use large redundant arrays of devices to sense the world. But if they're time consuming to configure, it makes them expensive.

Each device needs to connect to its local WiFi network, for which it needs credentials. Typically we get these credentials to the device using WPS or by turning the device into a WiFi access point running a captive config portal. WPS is insecure, and to be discouraged. A captive config AP portal on the device is insecure, annoying to use, and takes a lot of valuable memory. Hard-coding credentials into firmware is a non-starter for many reasons.

And then we have REST API keys, MQTT credentials, and some sense of device identity. All these need configuring per device too.

But there are some interesting things to note about all this...

1. Configuration is done only once or a few times during the entire life of the device
2. Most configuration parameters are uniform for any given network of devices; device specific settings are few

So why weigh down each device with all that baggage? Why not have one central configuration device, an access point to which client devices connect and download their configuration? We free up a lot of memory for use in important things like TLS and server verification. We also make configuration of large numbers of devices quick and easy. Plus, we get to reconfigure just as easily.

##ESP Configurator is born##

This is the idea behind ESP Configurator. A central device is the key to configuration of any number of devices. Once the Configurator device is switched off and put away (in a safe, if you like), there's no way into the devices. If you need to change network SSID, passcode or MQTT credentials, fire up the device once again, change the configuration centrally, and deploy that configuration to all devices within minutes.

The rules of engagement are thus:

1. device firmware has to use the ESP Configurator client library
2. the device SPIFFS filesystem gets encrypted - stored files become readable only on that device
3. the ESP Configurator Server device and client devices share a private key
4. a device enabled with ESP Configurator will only connect to one ESP Configurator server device
5. the ESP Configurator Server device checks each client for authenticity using 2FA

##JFDI Conceived ESP Configurator as Open Source##

JFDI Consulting conceived ESP Configurator to improve the security and configuration landscapes for ESP8266 and ESP32 devices. We want these devices and the incredible firmwares being written for them to be as secure and easy to configure as possible. We want the Espressif platform to become usable in a wider commercial/industrial context, without networks of sensors being ticking timebombs of vulnerability. We want to make sensor networks truly low-cost, not just to build or buy, but also to implement and maintain.

We could have kept this idea to ourselves... but we think it deserves to have broader scrutiny, a union of thinking, and greater opportunity for implementation in as many ESP devices as possible. So we've made it an Open Source project.

We invite individuals and companies to collaborate on this project with us. We intend for this project to become a defacto standard for ESP device security and configuration, freely available to anyone who wants to use it, for any purpose from personal projects to large-scale commercial/industrial implementations.
