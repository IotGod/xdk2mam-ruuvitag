# XDK2MAM package for RuuviTag with Ruuvi Station and Gateway

The following method will allow RuuviTag users to send sensor's data to the [Ruuvi Station](https://play.google.com/store/apps/details?id=com.ruuvi.station&hl=en) App on a mobile device and set a Gateway address on the app settings that will be in charge of publishing the datasets to th Tangle.

This approach has it pros and cons: while you do not need to deal with Noble to handle the BLE listener, you need an extra BLE capable device running the Ruuvi Station app to get the data and yet another instance to act as the Gateway that will get the app data to decentralize it using MAM.

So, let's start by downloading the [Ruuvi Station](https://play.google.com/store/apps/details?id=com.ruuvi.station&hl=en) app and removing the plastic piece from the beacon batterie to turn it on.
As soon as you start the app you will be able to add your beacon to see its sensors working live.

![Ruuvi Station App](https://xdk2mam.io/assets/images/ruuvitag_1.png)

This application gives the users the posibility to define a Gateway to which it will send the sensors data. Because we will be using our own computer as server to collect the app values, let's first setup the software required. 
Our mobile phone and computer are connected to the same WiFi, so our laptop LAN ip (192.168.1.10, you should use yours) would be provided as Gateway. But first, let's set the Node.js server.

Clone our repository

```
git clone https://github.com/xdk2mam/xdk2mam-ruuvitag.git
```

Done! Let's get into the **xdk2mam-ruuvitag-with-endpoint* folder to install the needed modules with the following command. 

```
npm i
```

Once the installation finishes, edit the server.js file to add your Full Node (be sure to use one with PoW enabled). We set one from [IOTA Dance](https://iota.dance) per default.

```
let iota = new IOTA({
  'provider': 'http://heimelaga.vodka:14265'
});
```
Once this modifications are done, save the file and let's run it to start our Node.js Gateway. 
Notice that the script **requires a parameter to indicate the interval defined in milliseconds in which the data will be published to the Tangle.** We will be doing so every 30 seconds.

```
node server.js 30000
```
Now that we have our server listening at 192.168.1.10, let's head to the Ruuvi Station settings to prepare the app.

First of all, we need to go into Background Scanning Settings to select the option **Continuous background scanning** which will keep getting data from the beacon and sending it to the defined Gateway even if we close the Ruuvi Station app.

![Ruuvi Station App Gateway](https://xdk2mam.io/assets/images/ruuvitag_2x.png)

To continue, we need to specify a Gateway so the app knows where to send the data. We head to App Settings and in **Gateway Settings** we enter our notebook IP with the port chosen at server.js and the endpoint /sensors. So, for our case it would be **192.168.1.10:8080/sensors**

![Ruuvi Station App Gateway](https://xdk2mam.io/assets/images/ruuvitag_3.png)


That's it! Check your console to see how the beacon sensor's data are printed together with the channel ids to verify that they are in deed published at the Tangle. First root is [HFKVRYBY9OOPZ9LEPZ9BUGQNCCBUTNZICHOLFNVCYQ9LXPXGYIBRXSIDFFMLFFONTCYRXVUFKDLWFOCMS](https://thetangle.org/mam/HFKVRYBY9OOPZ9LEPZ9BUGQNCCBUTNZICHOLFNVCYQ9LXPXGYIB) and has already been published to the Tangle as you can see if you follow the link.

![Data on Console](https://xdk2mam.io/assets/images/ruuvi_console_gateway.png)


