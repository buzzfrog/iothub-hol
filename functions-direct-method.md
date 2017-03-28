# Use Direct Method to initiate a command on a device #
## Introduction ##
This sample shows how to use a create a serverless function in *Azure Functions* that sends a command to a device through Iot Hub.

If you haven't already done the laboration 
[Consume events from IoT Hub and store them in DocumentDB](https://github.com/buzzfrog/iothub-hol/blob/master/iothub-save-to-docdb.md), you need to do section 1 and 2 in that document first.

## 1. Create a simulated device app

In this section, you create a Node.js console app that responds to a method called by the cloud.

1. Create a new empty folder called **simulateddevice**. In the **simulateddevice** folder, create a package.json file using the following command at your command prompt. Accept all the defaults:
   
    ```
    npm init -y
    ```
2. At your command prompt in the **simulateddevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Using a text editor, create a new **SimulatedDeviceWithMethod.js** file in the **simulateddevice** folder.
4. Add the following `require` statements at the start of the **SimulatedDeviceWithMethod.js** file:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Add a **connectionString** variable and use it to create a **DeviceClient** instance. Replace **{device connection string}** with the device connection string you generated in the *Create a device identity* section in the document [Consume events from IoT Hub and store them in DocumentDB](https://github.com/buzzfrog/iothub-hol/blob/master/iothub-save-to-docdb.md).
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Add the following function to implement the method on the device:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Open the connection to your IoT hub and start initialize the method listener:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Save and close the **SimulatedDeviceWithMethod.js** file.

## 2. Create an Azure Function

[Youtube: Create an Azure Function in Azure Portal](https://www.youtube.com/embed/a6BIybRjeIY)

## 3. Install dependencies for the function

[Youtube: Azure Functions, install dependencies](https://www.youtube.com/embed/M1c-RpFDInI)

It could take a couple of minutes to install all dependecies.

> NOTE
>
> The user interface for Azure function is rather new. This feature will hopefully be better supported in the future.

## 4. Update source code for the function

1. Replace all code in teh function with the following code: 

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    var connectionString = '{iot hub connection string}';
    var methodName = 'writeLine';
    var deviceId = '{device id}';

    module.exports = function (context, req) {
        context.log('JavaScript HTTP trigger function processed a request.');

        var client = Client.fromConnectionString(connectionString);

        var methodParams = {
            methodName: methodName,
            payload: 'a line to be written',
            timeoutInSeconds: 30
        };

        client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
            if (err) {
                context.log('Failed to invoke method \'' + methodName + '\': ' + err.message);
            } else {
                context.log(methodName + ' on ' + deviceId + ':');
                context.log(JSON.stringify(result, null, 2));
            }
        });

        context.res = {
            status: 200,
            body: ""
        };

        context.done();
    };
    ```

2. Replace **{iot hub connection string}** and **{device id}** with relevant information.

## 5. Test
1. Start the device simulator

    ```
    node SimulatedDeviceWithMethod
    ```
2. Run the Azure Function

3. Verify responses both on the command line and in the log window in the function user interface.

4. Copy the function url. You find it in the top of the user interface *Get function URL*.

5. Paste it into a browser and watch that we can call this function from internet.

[Youtube: Recording of the above test](https://www.youtube.com/embed/h6luu7r-Xpo)

