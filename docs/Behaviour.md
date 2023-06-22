# Behaviour
{:.no_toc}

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

<!-- _(c) AMWA 2023, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_  -->

The Annotation API provides access to the [annotation properties](Overview.md#core-resource-properties) associated with a particular resource, uniquely identified by its ID.

## Setting Values

The API allows clients to independently set values for `label`, `description` and individually named `tags`.

The following `PATCH` request to the `/node/self` endpoint asks to update the label and description but make no change to the tags associated with the Node resource.

```json
{
    "label": "fave node",
    "description": "my favourite node"
}
```

The following `PATCH` request to the `/node/devices/21a28338-fb2e-4df5-9b55-d58e6124bc9f` endpoint asks to add, or replace the values for, the "studio" tag but make no change to other tags, the label or description.

```json
{
    "tags": {
        "studio": ["HQ2"]
    }
}
```

## Resetting Values

The API allows clients to reset values for `label`, `description` and individually named `tags`, using `null`.

The following `PATCH` request asks to reset the label and "location" tag, at the same time as adding, or replacing the values for, the "studio" tag.

```json
{
    "label": null,
    "tags": {
      "location": null,
      "studio": ["HQ2"]
    }
}
```

The reset behaviour depends on the API implementation.
The new values MUST be valid per the schema.

* For labels and descriptions, the implementation MUST either restore an initial or configured default value, or set the value to the empty string.
* For a named tag, the implementation MUST either restore an initial or configured default array of values, or remove the named tag from the resource.

A `PATCH` request with `tags` set to `null` asks to reset all tags.

The following `PATCH` request asks to reset all the annotation properties for a resource.

```json
{
    "label": null,
    "description": null,
    "tags": null
}
```

## Read-Write Tags

The namespace `urn:x-nmos:tag:user:` is defined and included in the [Tags register](https://specs.amwa.tv/nmos-parameter-registers/branches/main/tags/) of the NMOS Parameter Registers.

API implementations MUST support read-write for tags in this namespace, subject to any internal limitations.

Tags in other namespaces can be read-write or read-only.

## Read-Only Tags

Some named tags, such as those defined by [BCP-002-01][] and [BCP-002-02][], or by a particular manufacturer, are intended to have read-only values assigned to the resources by the manufacturer.

An API implementation MAY therefore reject requests to update specific named tags, with a `500` (Internal Server Error) response.

A `PATCH` request with `tags` set to `null` does not update read-only tags.

## Additional Limitations

Some named tags have additional limitations, such as needing precisely one element in the array of values.
An API implementation SHOULD reject requests which do not meet the additional limitations specified for such tags, with a `500` (Internal Server Error) response.

Minimum requirements on supported annotations are specified in terms of size in Bytes when encoded in UTF-8.

* An API implementation MUST support writing a label of up to 64 Bytes for every resource.
* An API implementation SHOULD support writing a description of up to 64 Bytes for every resource.
* An API implementation MUST support writing at least 1 tag for every resource with:
  * a name of up to 64 Bytes with the URN prefix `urn:x-nmos:tag:user:`
  * an array of values with 1 element of up to 64 Bytes.
* An API implementation SHOULD support writing at least 5 such tags for every resource.
* An API implementation MAY support further annotation for some or all resources, for example based on the total size of annotations across all resources.

The API implementation MUST reject requests which it cannot process, with a `500` (Internal Server Error) response.

Notes:
* UTF-8 is a variable length encoding so 64 Bytes does not equate to a fixed number of code points or characters. The first 128 code points in the Basic Latin (ASCII) Unicode block (U+0000 to U+007F) need 1 Byte, the next 1920 code points (U+0080 to U+07FF) which cover the remainder of almost all Latin-script alphabets need 2 Bytes, and the remaining 61440 code points of the Basic Multilingual Plane (BMP), including most Chinese, Japanese and Korean characters (U+0800 to U+FFFF) need 3 Bytes. Code points in the Supplementary Planes (U+10000 to U+10FFFF) take 4 Bytes.
  The 64 Byte minimum requirement therefore corresponds to between 16 and 64 code points.
* Uniform Resource Names (URNs) are restricted to a subset of characters within the ASCII range by [RFC 8141][RFC-8141] and use the percent-encoding mechanism defined by [RFC 3986][RFC-3986] for octets outside this subset, which increases the number of Bytes taken to encode those characters. RFC 8141 recommends not using characters outside the ASCII range.

## Successful Response

When a successful `PATCH` request is made, the response MUST be the updated resource with the new values for all properties.

The `version` attribute is incremented in the response to the `PATCH` request and subsequent `GET` requests.
See the related [Interoperability: IS-04](Interoperability%20-%20IS-04.md#version-increments) section of the specification.

## Error Response

Unsuccessful requests MUST produce the error response per the [APIs](APIs.md#error-codes--responses) specification.
The API is strongly RECOMMENDED to include an informative error response body with `500` (Internal Server Error) responses mentioned above, to indicate why the request is rejected.

## Persistence of Updates

The API implementation MUST persist updates to the annotation properties for the lifetime of a resource uniquely identified by its ID, including consistency over reboots, power cycles, and software upgrades.

[BCP-002-01]: https://specs.amwa.tv/bcp-002-01 "BCP-002-01 Natural Grouping of NMOS Resources"
[BCP-002-02]: https://specs.amwa.tv/bcp-002-02 "BCP-002-02 NMOS Asset Distinguishing Information"
[RFC-3986]: https://tools.ietf.org/html/rfc3986 "RFC 3986: Uniform Resource Identifier (URI): Generic Syntax"
[RFC-8141]: https://tools.ietf.org/html/rfc8141 "RFC 8141: Uniform Resource Names (URNs)"
