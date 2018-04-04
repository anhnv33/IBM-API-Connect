# API Connect concept
High level understanding of the API management solution

### API Connect: Overview
API Connect provides a range of powerful capabilities through which you can discover or define your API, implement that API to connect with your existing backend services or create new microservices to expose data assets.

API Connect Deployment components:

![shot_180403_140535](https://user-images.githubusercontent.com/14268190/38234640-a747d9b4-3748-11e8-8034-d1acd401ad29.png)

Cross-components data flows:

![shot_180403_142858](https://user-images.githubusercontent.com/14268190/38235432-5fcc6156-374b-11e8-8656-a769939c1670.png)

#### User roles:
- Cloud Manager role - the system administrators who deploy and configure the infrastructure that makes up the solution. These users work mainly in the Cloud Manager user interface or with the appliance command-line.

- API Developer - design and implements the APIs that will be exposed through the product. Works mainly with the API connect toolkit installed on their laptop

- Product Manager - reponsible for publishing API Products to a Catalog when they are ready to be comsumed. Work mainly in the API Manager user interface on the Management server, or with the API Connect Toolkit command-line interface

- Community Manager - manages the set of Developer Organizations, Applications and Subscriptions that consume the published Products. Work mainly in the Community section of the Catalog in the API Manager user interface

- Application Developer - implements the Application that consumes the APIs. Discovers and subscribes to APIs through the Developer Portal and then writes the applications that invokes the APIs through API Gateway.

#### Relationship between Logical Concepts and Development Components

#### Environment Separation
It is common for customer to have a sequence of "environment" in their deployment process, with the expectation that the API infrastructure can support each of those steps in an appropriate fashion:
- Development
- Functional Test
- Performance or Load test
- Pre-production / staging
- Production

#### Recommended Environment Separation

#### Recommended Practice for API Development and Promotion
![image](https://user-images.githubusercontent.com/14268190/38245069-ea6a6f4a-3766-11e8-9800-2bddee3fbc0d.png)

The automated delivery pipeline typically includes the following steps as shown in this picture.
- API developer commits the file representing the changes they have made from their local filesystem to a source code control systems
- A plugin to the source code control system provides the ability to trigger a notification (or a job) when file changes are detected
- A job executes which extracts the source code onto the job server
- The job executes which extracts the source code onto the job server
- The job executes a script which uses the Developer Toolkit CLI to publish the Product to the target API Connect Cloud
- The job executes a series of functional verification tests
- If the test are successful the job might the execute the next job in the cycle, which might deploy and test the changes to the Pre-Production cloud

#### Typical Deployment Patterns
##### Minimal Development install
The first deployment most customer undertake is a small "development" installation.
![image](https://user-images.githubusercontent.com/14268190/38286851-bed9584c-37f1-11e8-8e75-b5dbb5521913.png)

This style of deployment is well suited (phù hợp) for Proof of Concept or simple functional validation scenarios in that it provides all the capability and functionality of the API Connect offering, without the need for the HA or resilience aspects.

##### Single Region with HA
![image](https://user-images.githubusercontent.com/14268190/38287745-836c1a6a-37f6-11e8-99da-681bbbd47799.png)

Adding high availability for API Connect within a single region is a simple case of deploying multiple instances of each component and configuring appropriate load balancing in
front of each cluster to route traffic around failures of any individual component as shown in the following diagram

- To provide HA you require a minimum of two servers per cluster but it í common to choose a minimum of three servers so that the failure of any single node still leaves two servers available to process the necessary traffic, which avoids overloading the single remaining server
- To successully handle the single-node-failure scenario you must ensure that under normal conditions each node has sufficient spare capacity to handle a share of traffic from the failed node(s). For example, in a 3-node deployment each node must nomally run at less than 66% CPU.

Deploying multiple nodes within a cluster provides good protection against the failure of an individual server process but we also need to consider other failure scenarios that may occur within a given region. For example, in a VMWare ESXi deployment each of the servers are a virtual machine running inside the ESXi server, so a failure of that ESXi server will affect all the servers in the cluster.

##### External and Internal API Exposure
Seperate Gateway service for internal and external traffic
![image](https://user-images.githubusercontent.com/14268190/38288767-d8cf4f1c-37fc-11e8-901c-a3b72512309c.png)

This approach is used successfully by customers who are comfortable with the following implications of the topology;

- There is a shared Management service and Developer Portal cluster so any problems in those tiers could affect both the external facing and internally facing users at the same time

Customer that are not comfortable with the contitions described above arising the shared Management service can choose to deploy two separate Clouds folowing diagram

![image](https://user-images.githubusercontent.com/14268190/38288980-05b68030-37fe-11e8-9c23-9ccec53b8349.png)

##### Dual Region with High availability
For on-premises customers the most common production deployment scenario is to have a single cloud clustered across two regions (DC)

![image](https://user-images.githubusercontent.com/14268190/38289030-5737a114-37fe-11e8-95fd-1a9baf419a48.png)

#### Key Points for Multi-Region Deployments
##### API Gateway
Attribute|Value
-----------------| ---------------
Session affinity | None - requests are stateless
Load Balancing | Round-robin for both global and local load Balancing
Persistence | Rate limit data is stored in-memory only - no Persistence
Replication | Rate limit information is replicated between peers using IP multicast
Failover behaviour | No failover required unless using AO for front-side load balancing
Deployment location | Suitable for DMZ deployment if desired
Cluster sizing | Varies in line with API traffic rate

##### User Interface Traffic
Attribute|Value
--------------|-------------------
Session affinity | Replication of user HTTP Session state exists for API Manager, Cloud Manager and Developer Portal for failover, but recommended to route back to same instance under normal conditions
Load Balancing | Support recommendation to route back to same instance by configuring both global and local load balancing for session affinity-based load balancing
Persistence | HTTP session replication is in-memory only
Replication | HTTP session replication takes places over TCP/IP
Failover behaviour | N/A
Deployment location | N/A
Cluster sizing | N/A

##### Management tier
Attribute|Value
--------------|-------------------
Session affinity | Replication of user HTTP Session state exists for API Manager, Cloud Manager and Developer Portal for failover, but recommended to route back to same instance under normal conditions
Load Balancing | Support recommendation to route back to same instance by configuring both global and local load balancing for session affinity-based load balancing
Persistence | Configuration data (APIs, Products, Apps, Subscriptions) in a persistence database. API call and audit analysis data stored in integrated ElasticSearch instance
Replication | Persistence database uses a primary + multiple secondary model, with fully copy replication. ElasticSearch is a shared replicated data store (partial copy on each node, where suitable)
Failover behaviour |
Deployment location | Private or protected zone. Not recommended for deployment in the DMZ
Cluster sizing | At least two (or three) nodes in each region

### Packaging strategy and terminology (thuật ngữ) in API Connect
#### APIVersion
An Application Programming Interface (API) is an industry-standard software technology.
An API is a set of routines, protocols, and tools for building software applications.

API are classified as external (public), partner (protected), or internal (private), based on how they are consumed.

An API is composed of operations, called methods, which are offered in one of the following styles in API Connect:
- A REST API is structured according to the principles (nguyên tắc) of Representational State Transfer. REST APIs use HTTP or HTTPS requests to PUT, GET, POST and DELETE data.

- A SOAP (Simple Object Access Protocol) API is a web service that is exposed as an API. SOAP interfaces are described in WSDL (Web Services Description Language) format. The WSDL is an XML document describing the structure for headers, messages, URL endpoints, and datatypes used to access a web service.

#### Plans and Products
Plans and Products are proprietary packaging constructs that are unique to API Connect. API providers use Products to offer one or more APIs to the application developers who will consume the APIs (API Consumers).

The providers use Plans to control access to APIs to manage API usage. Products are packages that contain both the APIs and the accompanying (kèm theo) Plans.

To make an API available to an application developer, it must be included in at least one Product and at least one Plan.

Plans perform the following functions:
- Control which APIs an application developer can use
- Make available a collection of operations form one or more APIs
- Apply rate limits to APIs to defferentiate between offerings
- Implement defferent rate limits to specify how many requests a consuming application is allowed to make during a specified time interval

A Product in API Connect bundles a set of APIs and Plans into one offering that is intended for a particular (riêng biệt) use. You can create Plans only within Products, and these Products are the published in a Catalog.

Relationship between Products and Plans:
- A Plan can belong to only one Product
- A Product can have multiple Plans that each contain a different set of APIs
- A Plan in one Product can share APIs with Plans from any other Product

![shot_180319_164909](https://user-images.githubusercontent.com/14268190/37588669-85033600-2b95-11e8-898e-d095e843b44c.png)

![shot_180319_164927](https://user-images.githubusercontent.com/14268190/37588694-96daf656-2b95-11e8-8f69-261f2d8d2804.png)

Products are used to manage the lifecycle of the APIs they contain. The states in the Product lifecycle include draft, staged, published, deprecated, retired, and archived.

#### Catalogs and Spaces
A Catalog contains a collection of Products. Catalogs are staging targets through which Products are published on a Developer Portal. Catalogs are used to seperate Products and APIs for testing before publishing them on a Developer Portal.

Each Catalog has an associated Developer Portal for exposing the published Products.

#### Organizations and users
There are two types of organizations: provider and developer. An organization can encompass(bao gồm) a project team, department, or division

Some standard reponsibilities (chuẩn trách nhiệm) within a provider organization include:
- A provider organization owner who has the full set of access permissions to API Connect functions, and can also commission (ủy nhiệm) APIs and track their business adoption.
- API developer who design and develop APIs, and applications for the provider organization to which they belong
- An administrator who manages the lifecycles of APIs and publishes API for discovery and use
- A "community" manager who manages the Relationship between the provider organization and organiztion developers, providers information about API usage, and provides support to application developers

Standard responsibilities within a developer organization include:
- A developer organization owner: add application developers, view Products and API

### APi Connect Component

### API Connect: End-to-end solution example
