# Consume events from IoT Hub and store them in DocumentDB #
## Introduction ##
This sample shows how to create a device simulator that pushes messages into IoT Hub and is consumed with an Azure Stream Analytics Job that stores the data into a DocumentDB.

## 1. Create the IoT Hub
[Youtube: Create IoT Hub in the Portal](https://www.youtube.com/embed/U8iku11V9oQ)

[Youtube: Create an IoT Hub with Azure CLI]()
The Azute CLI can be found [here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).
You need to authenticate with Azure before you can use the CLI

    az login

## 2. Create a device entity
Use the same *iothub-explorer* to register a device in the IoT Hub. Look at this [document](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-node-node-twin-getstarted) under the section *Create a device identity* to get a description of this.

Alt.

[Youtube: Create a device entity in the Portal]()


## 3. Create a simulated device with Node.Js
Use this [document](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-node-node-getstarted) under section *Create a simulated device app*.

Add a parameter to this line:

    var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
 to

     var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed, 'time': new Date().getTime() });

## 4. (test) Use iothub-explorer to monitor your device

    iothub-explorer monitor-events <device-id> --login <iot hub connection string>

Start your simulated device:

    node SimulatedDevice.js

You should receive messages, if all is ok.

## 5. Create a DocumentDB database

[Youtube: Create a DocumentDB database in the Portal]()

[Youtube: Create a DocumentDB database with Azure CLI]()

## 6. Create a Stream analytics job

[Youtube: Create a Stream analytics job]()

## 7. Start the simulator again
    node SimulatedDevice.js

## 8. Control that data is created in DocumentDB

[Youtube: Control that data is created in DocumentDB]()

