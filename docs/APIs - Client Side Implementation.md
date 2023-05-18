# APIs: Client Side Implementation Notes

_(c) AMWA 2023, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

## Failure Modes

If a client receives a HTTP `500` (Internal Server Error) response from the API, a failure has occurred.
The client SHOULD report the content of the response's `error` field to the User.
