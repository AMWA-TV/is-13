# Controllers

_(c) AMWA 2026, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

## Introduction

A Controller is Client software that interacts with the NMOS APIs to discover, connect and manage resources (Nodes, Devices, Senders and Receivers) within a networked media system.

- This document includes normative references to be followed when implementing a Controller that supports the Annotation API.
- This document covers how the Controller interacts with the NMOS APIs only.
  It does not cover other features of the Controller software, such as presentation.

Where this document refers to a User, this can include both human operators who drive a Controller manually and automation systems that drive a Controller programmatically.

## General

### HTTP APIs

#### Trailing Slashes

Controllers appending paths to `href` type attributes MUST support URLs both with and without a trailing slash, to avoid doubled or missing slashes.

Controllers performing requests other than `GET` or `HEAD` (e.g. `PATCH`, `PUT`, `POST`, `DELETE`, `OPTIONS`) MUST use URLs with no trailing slash present.

#### API Versions

The versioning format is `v<MAJOR>.<MINOR>`

- `MINOR` increments will be performed for non-breaking changes (such as the addition of attributes in a response)
- `MAJOR` increments will be performed for breaking changes (such as the renaming of a resource or attribute)

Versions MUST be represented as complete strings. Parsing MUST proceed as follows: separate into two strings, using the point (`.`) as a delimiter. Compare integer representations of `MAJOR`, `MINOR` version (such that v1.12 is greater than v1.5).

Implementers of Controllers are RECOMMENDED to support multiple versions of the NMOS APIs simultaneously in order to ease the upgrade process in live facilities.

#### Error Codes & Responses

The NMOS APIs use HTTP status codes to indicate success, failure and other cases to Controllers as per [RFC 7231](https://tools.ietf.org/html/rfc7231) and related standards.

As explicit handling of every possible HTTP response code is not expected, Controllers MUST implement generic handling for ranges of response codes (`1xx`, `2xx`, `3xx`, `4xx` and `5xx`).
However, where the API specifies explicit response codes the Controller SHOULD handle these cases explicitly.

When a Controller performs `GET` and `HEAD` requests, it MUST correctly handle a `301` (Moved Permanently) redirect to accommodate the API implementations permitted in the [APIs: URLs: Approach to Trailing Slashes](APIs.md#urls-approach-to-trailing-slashes) section of this specification.

If a Controller receives an HTTP `5xx` or `4xx` response code from the API, a failure has occurred.
The Controller SHOULD display the content of the response's `error` field to the User if possible.
For example, a Node can reject an update to a read-only tag, or a value which exceeds its internal limitations, as described in the [Behaviour](Behaviour.md) section of this specification.

## Annotation

The Controller MUST discover resources as specified in [IS-04: Controllers](https://specs.amwa.tv/is-04/releases/v1.3.2/docs/Controllers.html), and MUST identify the Nodes which implement the Annotation API as described in the [Discovery](#discovery) section below.

- The Controller MUST support annotation of Senders and Receivers.
- The Controller SHOULD support annotation of Nodes and Devices.
- The Controller MAY support annotation of Sources and Flows.

For each resource of a supported type that is included in the Annotation API of a Node, the Controller MUST allow the User to:

- set the `label`
- set the `description`
- set a named tag in the `urn:x-nmos:tag:user:` namespace to a single value, as described in the [Behaviour: Read-Write Tags](Behaviour.md#read-write-tags) section of this specification
- reset each of these properties, by setting it to `null` as described in the [Behaviour: Resetting Values](Behaviour.md#resetting-values) section of this specification

The Controller updates these properties using the Annotation API, as described in the [Behaviour](Behaviour.md) section of this specification.

The Controller SHOULD include in each `PATCH` request only the annotation properties, and only the named tags, that the User asks to update.
This avoids overwriting changes made by another Controller since the Controller last retrieved the resource.

When a `PATCH` request succeeds, the response contains the updated resource.
The values in the response can differ from those requested, for example after a reset.
The Controller reflects the values held by the Node to the User as specified in the [Version Increments](#version-increments) section below.

## Client Side Implementation Notes

Controllers MUST adhere to the [APIs: Client Side Implementation Notes](APIs%20-%20Client%20Side%20Implementation.md) described in this specification, such as the sections on Conforming to Schemas and Failure Modes.

## Interoperability with IS-04

### API Common Keys

Controllers MUST follow the requirements for common API keys specified in the [IS-04 APIs: Common Keys](https://specs.amwa.tv/is-04/releases/v1.3.2/docs/APIs_-_Common_Keys.html) document including the requirements regarding [use of URNs](https://specs.amwa.tv/is-04/releases/v1.3.2/docs/APIs_-_Common_Keys.html#use-of-urns).

### Discovery

The Controller MUST identify the Nodes which implement the Annotation API from the `services` array of each Node, as described in the [Interoperability: IS-04: Discovery](Interoperability%20-%20IS-04.md#discovery) section of this specification.
The Controller MUST use the `href` of a `services` entry for a version of the Annotation API that it supports to make requests to the API.

### Version Increments

When any Controller changes the annotation properties of a resource via the Annotation API, the Node also updates the corresponding IS-04 resource, as described in the [Interoperability: IS-04: Version Increments](Interoperability%20-%20IS-04.md#version-increments) section of this specification.

The Controller MUST reflect changes to the annotation properties of resources of a supported type to the User after a maximum of 30 seconds.

In Registered Operation, to avoid polling of the HTTP API the Controller SHOULD use an IS-04 Query API WebSocket subscription to monitor version timestamp increments on those resources.

## TCP Sessions

The Controller SHOULD use persistent TCP connections wherever possible,
to allow multiple operations to be performed without repeatedly closing and reopening connections for each operation.

If the Controller uses HTTP pipelining, the guidelines set out in [RFC 7230 section 6.3.2](https://datatracker.ietf.org/doc/html/rfc7230#section-6.3.2) apply.
