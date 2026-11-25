# APIs: Client Side Implementation Notes

_(c) AMWA 2023, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

## Conforming to Schemas

The body of each `PATCH` request made by a client MUST be valid according to the [`resource_core_patch.json`](../APIs/schemas/resource_core_patch.json) schema, which the [RAML](../APIs/AnnotationAPI.raml) specifies for these requests.

## Failure Modes

If a client receives a HTTP `500` (Internal Server Error) response from the API, a failure has occurred.
The client SHOULD report the content of the response's `error` field to the User.
