# Behaviour
{:.no_toc}

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

<!-- _(c) AMWA 2023, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_  -->

The Read/Write Node API provides access to the [core information properties](Overview.md#core-resource-properties) associated with a particular resource, uniquely identified by its ID.

## Setting Values

The API allows clients to independently set values for `label`, `description` and individually named `tags`.

The following `PATCH` request to the `/self` endpoint asks to update the label and description but make no change to the tags associated with the Node resource.

```json
{
    "label": "fave node",
    "description": "my favourite node"
}
```

The following `PATCH` request to the `/devices/21a28338-fb2e-4df5-9b55-d58e6124bc9f` endpoint asks to add, or replace the values for, the "studio" tag but make no change to other tags, the label or description.

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

* For labels and descriptions, the implementation MAY restore an initial or configured default value, or set the value to the empty string.
* For a named tag, the implementation MAY restore an initial or configured default array of values, or remove the named tag from the resource.

A `PATCH` request with `tags` set to `null` asks to reset all tags.

The following `PATCH` request asks to reset all the core information properties for a resource.

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

Some named tags, such as those defined by [BCP-002-01][] and [BCP-002-02][], or by a particular manufacturer, are intended to have read-only values assigned to the resources by the manufactuer.

An API implementation MAY therefore reject requests to update specific named tags, with a `500` Internal Error response.

A `PATCH` request with `tags` set to `null` does not update read-only tags.

## Additional Limitations

Some named tags, have additional limitations, such as needing precisely one element in the array of values.
An API implementation SHOULD reject requests which do not meet the additional limitations specified for such tags, with a `500` Internal Error response.

An API implementation MAY have additional limitations such as:
- maximum length of strings used as labels, descriptions or tag values
- maximum number of values for each named tag
- maximum number of tags
- maximum total size of information per resource or across all resources

The API implementation MAY reject requests which it cannot process, with a `500` Internal Error response.

## Successful Response

When a successful `PATCH` request is made, the response MUST be the updated resource with the new values for all properties.

The `version` attribute is incremented in the response to the `PATCH` request and subsequent `GET` requests.
See the related [Interoperability: IS-04](Interoperability%20-%20IS-04.md#version-increments) section of the specification.

## Error Response

Unsuccessful requests MUST produce the error response per the [APIs](APIs.md#error-codes--responses) specification.
The API is strongly RECOMMENDED to include an informative error response body with `500` Internal Error responses mentioned above, to indicate why the request is rejected.

## Persistence of Updates

The API implementation MUST persist updates to the core information properties for the lifetime of a resource uniquely identified by its ID, including consistency over reboots, power cycles, and software upgrades.

[BCP-002-01]: https://specs.amwa.tv/bcp-002-01 "BCP-002-01 Natural Grouping of NMOS Resources"
[BCP-002-02]: https://specs.amwa.tv/bcp-002-02 "BCP-002-02 NMOS Asset Distinguishing Information"
