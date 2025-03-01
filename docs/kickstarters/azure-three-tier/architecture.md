---
id: azure-three-tier-architecture
title: Architecture
---

# Architecture

## Introduction

This architecture designed for a sample To-do application is based on a three-tier architecture pattern consisting of a presentation layer, an application layer and a data layer. Three-tier pattern provides separation of concerns. The front-end, the web application that the user interacts with is the presentation layer. The business logic, the business rules and the communication between the presentation layer and the database belongs to the application layer. The data layer consists of the database.

This architecture is designed to mainly optimize the availability, scalability and security of any application that utilizes this architecture. This sample architecture can be used to implement any scalable and secure application that is highly available. 

The web application is deployed as an Azure Web App and the Azure Active Directory B2C authenticates the users of the application. Azure SQL database is used as the database layer of this architecture. The public access to the database is not allowed but the with the enabled service endpoint, all the resources inside the virtual network will have access to the database. App Service Regional VNet Integration enables the backend and the database of the application to communicate through the virtual network. Azure Bastion service enables secure direct access to the environment and the database.   

The high-level architecture diagram is given below.

![architecture](https://raw.githubusercontent.com/KamalRathnayake/architecture.99x.io/master/docs/kickstarters/azure-three-tier/architecture.JPG)

[Diagram goes here]

## Deployment View

### Components

**App Service**

Azure App Service is a fully managed service for building, deploying, and scaling web apps. The virtual network integration feature of App Service can give your app access to resources in or through a virtual network.

**Azure Active Directory**

Azure Active Directory (Azure AD) is a cloud-based identity and access management service.

**Virtual Network**

Azure Virtual Network is a secure private network in the cloud. It connects Azure Resources to one another, to the internet, and to on-premises networks.

**Azure Bastion**

Azure Bastion is a fully managed service that provides more secure and seamless Remote Desktop Protocol (RDP) access to virtual machines (VMs) without any exposure through public IP addresses.

**Azure SQL**

Azure SQL Database is a fully managed platform as a service (PaaS) database engine that handles most of the database management functions.


### Workflows

Here's the traffic flow and basic configuration of the architecture:

1. Requests route from the internet to a front-end app.
2. Azure AD B2C is used for the authentication. Only authenticated users can access the application.
3. The traffic from the backend API to the database is routed through the virtual network. App Service regional VNet integration is enabled for this purpose. 
4. Azure SQL database is used for storing the data. The resouces in the virual netowork can access the database as we have enabled service endpoint connectivity for Azure SQL databases. And the public internet access is prohibited. 
5. Azure Bastion service is used for accessing the environment and the database securely. 


**Why did we use Azure AD B2C?**

**Why did we use a Virtual Network/VNET integration?**

We are introducing a virtual network to the architecture to make the communication secure between the App Service and the database. The App Service is integrated with the VNet using App Service regional VNet integration feature. Mainly due to increased flexibility when managing the future complexity and security of the application.

**Why did we use service endpoints?**
We're going with Azure SQL the PaaS database platform on Azure as the data storage solution mainly due to it's wide use and extensibility of this template. To enable the communication from the VNet to the database we have two features provided by Azure.

1. Private Endpoint Connections
2. Service Endpoint Connections

A private endpoint is a powerful feature that provides the PaaS service representation in the VNet as we virtual NIC that makes it possible to access the PaaS service (In this case Azure SQL database) with even VPN connections. But the con here is private endpoints are costly and for our requirements just having secure access to the database is enough won't require VPN connections to the database as well. In considering all this, We have decided to go ahead with service endpoint connectivity feature.

**Why did we use Azure Bastion?**

## Development View
Technologies used for developing
ASP.NET MVC, 