# AMWA NMOS Read/Write Node Specification: Overview \[Work In Progress\]
{:.no_toc}

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

<!-- _(c) AMWA 2023, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_  -->

## Introduction

AMWA IS-13 specifies how to update distinguishing information in an NMOS system.

- The [Read/Write Node API](../APIs/ReadWriteNodeAPI.raml) is used to update resource labels, descriptions and tags on a Node.

The Specification includes:

- RAML and JSON Schema definitions, with supporting JSON examples
- This documentation set, which provides:
  - An overview of the API and how it is used.
  - Normative requirements in addition to those included in the RAML and JSON schemas specifying the API.
  - Additional details and recommendations for implementers of API providers and clients.
  - Information about interoperability with other specifications and compatibility between different API versions.

IS-13 is intended to be implemented on Nodes that support [IS-04 NMOS Discovery & Registration](https://specs.amwa.tv/is-04), but it does not depend on such support.

The [NMOS Glossary][Glossary] defines several common terms that have specific meanings in NMOS.

## Use of Normative Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY",
and "OPTIONAL" in this documentation set are to be interpreted as described in [RFC 2119][RFC-2119].

## Terminology

### Resources

IS-13 uses the same set of resource types and related API endpoints as IS-04.
That is a Node resource at the `/self` endpoint, and a number of Devices, Sources, Flows, Senders and Receivers at `/{resourceType}/{resourceId}`.

### Core Resource Properties

In the Read/Write Node API, each resource provides only the core identity and information properties.
That is, the `id`, `version`, `label`, `description` and `tags`.
These properties are described in the IS-04 [APIs: Common Keys](https://specs.amwa.tv/is-04/releases/v1.3.2/docs/APIs_-_Common_Keys.html) section.

The `id` provides persistent identity for resources and the `version` identifies the instant at which a resource most recently changed.
These are assigned by the node implementing the API.

The `label`, `description` and `tags` of each resource have initial values assigned by the node.
IS-13 enables these core information properties to be updated by a client with a `PATCH` request, subject to a node's internal limitations.

[Glossary]: https://specs.amwa.tv/nmos/main/docs/Glossary.html "NMOS Glossary"
[RFC-2119]: https://tools.ietf.org/html/rfc2119 "Key words for use in RFCs"
