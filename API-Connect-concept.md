# API Connect concept
High level understanding of the API management solution

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
- A Product can have multiple Plans that each contain a defferent set of APIs
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
