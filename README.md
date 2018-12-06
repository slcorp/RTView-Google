# RTView-Google

The RTView-Google repo provides information about deploying RTView products into the Google Cloud Platform.

RTView is a real-time data management, visualization and analytics engine provided by SL Corporation (www.sl.com). It is used by organizations worldwide as a key component of mission-critical monitoring and control systems built around various middleware, infrastructure and IoT data sources.

The Google Cloud Platform (GCP) is a suite of cloud computing services that runs on the same infrastructure that Google uses internally for its end-user products. This platform makes it convenient and easy to deploy service components of any application into the Google Cloud. 

The instructions provided here show how to install, configure, and run an RTView DataServer on GCP.
The RTView DataServer is the data management component that provides in-memory caching and archival to persistent storage. Data stored there may be consumed by displays, dashboards, reports and alerts provided by a visualization and analytics tool such as RTView Cloud.

There are at least three distinct ways in which RTView can be deployed to GCP. These are described in the sections below.

## Compute Engine

The Google Cloud Marketplace offers a single-click installation of an RTView DataServer, fully configured, and available for immediate use. It is a Virtual Machine deployment, running on the Google Compute Engine, and billed automatically based on usage. Note that there may be multiple variants of the RTView DataServer provided, each designed for a specific purpose. Instructions for all are essentially the same.

Clicking into the ComputeEngine folder of this repository will take you to the instructions for deploying and making use of the RTView DataServer instance that you create.

## Kubernetes

In some cases, there may be a need to deploy RTView with a configuration that is different from the defaults provided in the ComputeEngine version.

In this case, it is recommended that you use the Kubernetes deployment model and build your desired configuration using the instructions provided by clicking to the Kubernetes folder of this repo. Kubernetes is a container orchestration tool, offered in Google Cloud. It can deploy and automatically integrate multiple services into complex applications.

## Docker Container

Often, users simply want to deploy a simple RTView container into Google Cloud and have total control over the management of the Docker containers themselves.  This section explains how to do this for RTView running in Google Cloud.


