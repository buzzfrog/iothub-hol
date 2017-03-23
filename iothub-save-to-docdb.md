# Consume events from IoT Hub and store them in DocumentDB #
## Introduction ##
This sample shows how to create a device simulator that pushes messages into IoT Hub and is consumed with an Azure Stream Analytics Job that stores the data into a DocumentDB.

## 1. Create the IoT Hub
[Youtube: Create IoT Hub](https://www.youtube.com/embed/U8iku11V9oQ)

## 2. Create a device entity
Use the same *iothub-explorer* to register a device in the IoT Hub. Look at this [document](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-node-node-twin-getstarted) under the section *Create a device identity* to get a description of this.

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
We have several options, but in this tutorial we use the  CLI `az`. It can be found [here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

### 5.1 Authenticate
    az login

### 5.2 Create a Resource group
    az group create --name buzzgroup --location "North Europe"

### 5.3 Create the DocumentDB database
    az documentdb create --name buzzdocdb --resource-group buzzgroup




