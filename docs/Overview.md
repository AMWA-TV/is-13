# AMWA NMOS Read/Write Node Specification: Overview
{:.no_toc}

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

_(c) AMWA 2021, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

## Introduction

AMWA IS-xx specifies how to update distinguishing information in an NMOS system.

- The [Read/Write Node API](../APIs/ReadWriteNodeAPI.raml) is used to update resource labels, descriptions and tags on a Node.

The Specification includes:

- RAML and JSON Schema definitions, with supporting JSON examples
- This documentation set, which provides:
  - An overview of the API and how it is used.
  - Normative requirements in addition to those included in the RAML and JSON schemas specifying the API.
  - Additional details and recommendations for implementers of API providers and clients.
  - Information about interoperability with other specifications and compatibility between different API versions.

IS-xx is intended to be implemented on Nodes that support [IS-04 NMOS Discovery & Registration](https://specs.amwa.tv/is-04), but it does not depend on such support.

The [NMOS Glossary][Glossary] defines several common terms that have specific meanings in NMOS.

## Use of Normative Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY",
and "OPTIONAL" in this documentation set are to be interpreted as described in [RFC 2119][RFC-2119].

## Requirements on Nodes

A Node MUST implement the [Read/Write Node API](../APIs/ReadWriteNodeAPI.raml).

[Glossary]: https://specs.amwa.tv/nmos/main/docs/Glossary.html "NMOS Glossary"
[RFC-2119]: https://tools.ietf.org/html/rfc2119 "Key words for use in RFCs"
