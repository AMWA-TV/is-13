# Interoperability: IS-04

_(c) AMWA 2017, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

The Read/Write Node API shares a data model with the IS-04 specification, and as a result it is designed to be used alongside it. The following sub-sections identify correct behaviour for doing this.

When this API is used alongside IS-04 in a deployment, the IS-04 APIs SHOULD be operating at version 1.1 or greater in order to ensure full interoperability.

## Discovery

The Read/Write Node API SHOULD be advertised as a service endpoint.
Control interfaces can identify all Nodes which implement the Read/Write Node API by a `type` Uniform Resource Name (URN) having the base `urn:x-nmos:service:rw-node` followed by a `/` character and the API version.
The associated `href` is the URL of the Read/Write Node API base resource.

**Example:** The `services` attribute of an NMOS Node

```json
...
"services": [
  {
    "type": "urn:x-nmos:service:rw-node/v1.0,
    "href": "http://192.168.10.3/x-nmos/rwnode/v1.0/"
  }
]
...
```

As shown above the API version is included in the `type`, and in the `href`. Further service endpoints for the Read/Write Node API MAY be advertised for Nodes which support multiple versions simultaneously, or have multiple network interfaces.