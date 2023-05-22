# Interoperability: IS-04

_(c) AMWA 2023, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

The Annotation API shares a data model with the IS-04 specification, and as a result it is designed to be used alongside it. The following sub-sections identify correct behaviour for doing this.

When this API is used alongside IS-04 in a deployment, the IS-04 APIs SHOULD be operating at version 1.1 or greater in order to ensure full interoperability.

## Discovery

The Annotation API SHOULD be advertised as a 'service' endpoint under an IS-04 NMOS Node in the `services` array.
Control interfaces can identify all Nodes which implement the Annotation API by a `type` Uniform Resource Name (URN) having the base `urn:x-nmos:service:annotation` followed by a `/` character and the API version.
The associated `href` is the URL of the Annotation API base resource.

**Example:** The `services` attribute of an NMOS Node

```json
...
"services": [
  {
    "type": "urn:x-nmos:service:annotation/v1.0",
    "href": "http://api.example.com/x-nmos/annotation/v1.0/"
  }
]
...
```

As shown above the API version is included in the `type`, and in the `href`. Further service endpoints for the Annotation API MAY be advertised for Nodes which support multiple versions simultaneously, or have multiple network interfaces.

## Consistent Resources

When used in conjunction with IS-04, the UUIDs of [Node Resources](Overview.md#node-resources) in the Annotation API MUST match those in the corresponding IS-04 Node API.
The [Core Resource Properties](Overview.md#core-resource-properties) of the resources under the Annotation API `/node/` path MUST also match the corresponding IS-04 properties.

## Version Increments

In order to prevent unnecessary polling of the APIs, changes to any resource properties via the Annotation API also update the corresponding IS-04 resource and are therefore signalled via the IS-04 versioning mechanism.
When a successful `PATCH` request is made, the `version` attribute is incremented.
The `version` reported in the Annotation API is also updated whenever the corresponding IS-04 resource is updated.
