# RTView-Google

The RTView-Google repo provides information about deploying RTView products into the Google Cloud Platform.

RTView is a real-time data management, visualization and analytics engine provided by SL Corporation (www.sl.com). It is used by organizations worldwide as a key component of mission-critical monitoring and control systems built around various middleware, infrastructure and IoT data sources.

The Google Cloud Platform (GCP) is a suite of cloud computing services that runs on the same infrastructure that Google uses internally for its end-user products. This platform makes it convenient and easy to deploy service components of any application into the Google Cloud. 

The instructions provided here show how to configure, install, and run an RTView DataServer on GCP.
The RTView DataServer is the data management component that provides in-memory caching and archival to persistent storage. Data stored there may be consumed by displays, dashboards, reports and alerts provided by a visualization and analytics tool such as RTView Cloud.

There are at least three distinct ways in which RTView can be deployed to GCP. These are described in the sections below.

## Compute Engine

Thsi describes how to deplopy quickly to compute engine and obtain a completely enabled RTView Data Server.

## Kubernetes

If you want to use Kubernettes, details provided here. REferences base information contained in the RTView-Kubernetes repo.

## Docker Container

Sometimes user simply want to deploy a simple RTView container in Google.  This section explains how to do that.


