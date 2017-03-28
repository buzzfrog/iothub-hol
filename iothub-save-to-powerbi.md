# Consume events from IoT Hub and visualize them in PowerBI #
## Introduction ##
This sample shows how to create a device simulator that pushes messages into IoT Hub and is consumed with an Azure Stream Analytics Job that forward the data into PowerBI.

If you haven't already done the laboration 
[Consume events from IoT Hub and store them in DocumentDB](https://github.com/buzzfrog/iothub-hol/blob/master/iothub-save-to-docdb.md), you need to do section 1 to 4 in that document first.



## 1. Create a Stream analytics job

[Youtube: Create a Stream analytics job that send its output to PowerBI](https://www.youtube.com/embed/5KFikpBXyZ0)

> NOTE
>
> Make sure that the Stream Analytics job has started and is in the state **Running**.

## 2. Start the simulator again
    node SimulatedDevice.js

## 3. Control that data is created in DocumentDB

[Youtube: Create a PowerBI real time visualization](https://www.youtube.com/embed/V91svpSSCu4)

