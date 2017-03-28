# Consume events from IoT Hub and store them in DocumentDB #
## Introduction ##
This sample shows how to create a device simulator that pushes messages into IoT Hub and is consumed with an Azure Stream Analytics Job that stores the data into a DocumentDB.


## 1. Create the IoT Hub
[Youtube: Create IoT Hub in the Portal](https://www.youtube.com/embed/U8iku11V9oQ)

Or

Create an IoT Hub with Azure CLI

The Azute CLI can be found [here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

>You need to authenticate with Azure before you can use the CLI
>
>    az login

    az group create --name <resoure-group-name> --location "West Europe"

    az iot hub create --name <iothub name> --resource-group <resoure-group-name> --sku s1
> It can take a couple of minutes to create the iot hub.

> NOTE
>
>You will need the Iot Hub Connection String later. It could be good to copy it right now
so it is easy to find later.

> You find it in the Azure Portal by opening the **IoT Hub** pane and under **settings**, click on **Shared access policies** and choose **iothubowner**. 

## 2. Create a device entity
[Youtube: Create a device entity in the Portal](https://www.youtube.com/embed/Pu3tO4awXW0)

Or

Use the same *iothub-explorer* to register a device in the IoT Hub. Look at this [document](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-node-node-twin-getstarted) under the section *Create a device identity* to get a description of this.

> NOTE
>
> You need the *Device Id* and *Primary Key* later. You can copy it right now so it easier to find later.

## 3. Create a simulated device with Node.Js
This is copied from the this [document](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-node-node-getstarted) under section *Create a simulated device app* and changed in a couple small ways.

### Create a simulated device app
In this section, you create a Node.js console app that simulates a device that sends device-to-cloud messages to an IoT hub.

1. Create an empty folder called **simulateddevice**. In the **simulateddevice** folder, create a package.json file using the following command at your command prompt. Accept all the defaults:
   
    ```
    npm init
    ```
2. At your command prompt in the **simulateddevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Using a text editor, create a **SimulatedDevice.js** file in the **simulateddevice** folder.
4. Add the following `require` statements at the start of the **SimulatedDevice.js** file:
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. Add a **connectionString** variable and use it to create a **Client** instance. Replace **{youriothostname}** with the name of the IoT hub you created the *Create an IoT Hub* section. Replace **{yourdevicekey}** and **{deviceid}** with the device key and id values you generated in the *Create a device identity* section:
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={deviceid};SharedAccessKey={yourdevicekey}';
   
    var client = clientFromConnectionString(connectionString);
    ```

The {youriothubhostname} is easiest to find in the IoT Hub connection string that you copied before. 

HostName=**buzz4iothub.azure-devices.net**;SharedAccessKeyName=iothubowner;SharedAccessKey=9WcyDeRoBFahlJwswQhgLGGCo6+iNaWGFpRRoETnOdM=

Example:

    var connectionString = 'HostName=buzz4iothub.azure-devices.net;DeviceId=dev100;SharedAccessKey=be1CMCLV2BMsVV2ii2IcR2QqNJlxMXVNuBwtF8GuOIc=';

6. Add the following function to display output from the application:
   
    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Create a callback and use the **setInterval** function to send a message to your IoT hub every second:
   
    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
   
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed, 'time': new Date().getTime() });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
8. Open the connection to your IoT Hub and start sending messages:
   
    ```
    client.open(connectCallback);
    ```
9. Save and close the **SimulatedDevice.js** file.

## 4. (test) Use iothub-explorer to monitor your device
    iothub-explorer monitor-events <device-id> --login <iot hub connection string>

Start your simulated device:

    node SimulatedDevice.js

You should receive messages, if all is ok.

## 5. Create a DocumentDB database

[Youtube: Create a DocumentDB database in the Portal](https://www.youtube.com/embed/279XT_mgLp0)

Or

    az documentdb create --name <documentdb-name> --resource-group <resoure-group-name>
 > It can take a couple of minutes to create the documentdb database   

## 6. Create a Stream analytics job

[Youtube: Create a Stream analytics job](https://www.youtube.com/watch?v=mKLi9V4BA4M)

## 7. Start the simulator again
    node SimulatedDevice.js

## 8. Control that data is created in DocumentDB

[Youtube: Control that data is created in DocumentDB](https://www.youtube.com/embed/zmI0obC4jD0)

