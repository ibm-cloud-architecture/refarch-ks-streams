# K Container Shipment Use Case: IBM Streaming Analytics Application

This streaming application demonstrates real time event processing applied to inputs from
the ships and containers used in the K Container Shipment Use Case.  It is designed to monitor
refrigerated containers, known as reefers, stowed on ships which are traveling over blue water.
If an error condition is detected, for example a fire has started, an event is sent out to notify
the external services of the condition.

## Table Of Contents

* [What You Will Learn](#what-you-will-learn)
* [Application](#application)
* [Development Environment](#development-environment)
* [IBM Cloud Streaming Analytics Instance](#ibm-cloud-streaming-analytics-instance)
* [Build and Execute the Application](#build-and-execute-the-application)
* [References](#references)

## What You Will Learn
- How to implement a basic application with IBM Streaming Analytics.
- How to use business logic and machine learning models on real time data.
- The steps needed to set up and manage the IBM Streaming Analytics Service on IBM Cloud.
- How to use configure and use python for the application development. 

## Application

### User Stories 

- [ ] As a Shipping Agent, I’d like to understand the health of and manage the operations of reefer (refrigerated) containers in transit, to ensure that I am effectively protecting goods in my care, and managing cost.
- [ ] As a Shipping Agent, I need to understand when  a container isn’t operating within normal boundaries and automatically take corrective action.
- [ ] As a Shipping Agent, I’d like to understand when a container temperatures are trending towards a boundary and may need a reset.
- [ ] As a Shipping Agent, I’d like to understand when containers may have a potential failure so I can proactively take action to protect goods in transit.
- [ ] As a Shipping Agent, I’d like to understand when a container is failing so I can take corrective action.
- [ ] As a Shipping Agent, I’d like to automatically manage container settings based on any course or route deviations or rerouting events.

### Structure

![](streams-app.png)

### Ingest: Data Inputs

The application takes in a continuous stream of real time data which is used to make decisions and predict possible problems.  The data enters the streaming analytics application as a tuple which includes all of the following items:
- Measurement timestamp.
- Ship identifier.
- Ambient temperature of the ship.
- Current ship location as a latitude / longitude pair.
- Internal temperature of each container.
- Average watt-hours of power consumption since the last measurement was taken.

### Prepare: Transform the Inputs

As input tuples arrive, a streaming application must examine the data and prepare it for
use downstream in the application.  For this example, the following steps are taken:
- Fill in missing readings with extrapolations from past data points.
- Aggregate data into widows so that the data is smoothed and missing samples are handled.
- Reject late arriving data.
- Remove and report invalid data points.

### Detect, Predict, and Decide

This stage is responsible for applying business logic to the stream of data.  This logic can consist of 
simple bounds checks, complex rules, machine learning models, etc.  For the KC application we have 
initially implemented simple bounds checks, but will move to a more complex machine learning model in
order to illustrate a more advanced application.

Currently, the application includes the following simple checks: 
1. Temp is above threshold and power consumption is above threshold ==> Failure 

TODO: Add more complete rules and models here.

### Act

TODO.

## Development Environment

Application development can be done on any Linux system with the necessary packages installed.
Build is performed with a cloud based service and the application execution also occurs on a 
cloud based service.  Because this application leverages cloud services, there is no need to
install IBM Streams on your development system.

### Prerequisites

The application has been written and validated with Python version 3.5, so that version is recommended
for running this application.

First, ensure that Python 3.5 is installed on your system:
```bash
python3.5 --version
```
If it is not installed, follow the process described by your OS vendor.  As an example, for CentOS:
```bash
sudo yum install python35u
```

Next you will need to set up a Python 3.5 virtual environment, as follows: 
```bash
virtualenv --python=python3.5 .venv
source .venv/bin/activate
python --version
```
Some open source packages for IBM Streams are required for this application.  
In some cases, specific versions have been selected for compatibility reasons.
To install these packages, run the follow commands from your python virtual environment.

```bash
pip install streamsx
pip install --upgrade streamsx==1.11.3a0
pip install streamsx.messagehub
pip install redis 
```
## IBM Cloud Streaming Analytics Instance

### Create the Environment

TODO: describe creating the cloud environment here. 

### Manage the Environment

TODO: describe managing the cloud environment here. 

## Build and Execute the Application

The following script performs the application build and submits it to the IBM Cloud Streaming Analytics service.  Once it has been successfully run, the application will be running on the cloud watching for input events and producing output events in response on the Event Streams buses.

```bash
ReeferMonRun.sh
```

# Items that will be depricated and removed.

SmokeTest for all the components. Sends EGK data at 1sec intervals. 
```bash
SmokeTestEKG.py
```
 
Run the following script to start the simulator:
```bash
SimulatorRun.sh
```

## References 
 - [Developing IMG Streams Applications with Phython](http://ibmstreams.github.io/streamsx.documentation/docs/python/1.6/python-appapi-devguide/index.html)
-  [Streams Python Tutorial](https://developer.ibm.com/courses/all/streaming-analytics-basics-python-developers/)  
 - [Streamsx Documentation](https://pypi.org/search/?q=streamsx)
 - [IBM Streams Documentation](http://ibmstreams.github.io/streamsx.documentation/) 
 - [IBM Streams Python Support](https://streamsxtopology.readthedocs.io/en/latest/index.html)
 - [Ship Short Location ](https://www.navcen.uscg.gov/?pageName=AISMessagesA)
 - [Ship Long Location ](https://www.navcen.uscg.gov/?pageName=AISMessage27)

