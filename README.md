# Leadosaurus OAuth Proxy Service

This service is meant to be the OAuth bridge between Leadosaurus and other services. Without the need to establish our own infrastructure, Pizzly securely handles the OAuth dance that's often required when working with APIs for server to server interaction.

## Usage

1. When dealing with OAuth access to a particular service, generate a `client id`, a `client secret` and `scopes` on their end. Once that's generated, use those credentials within Pizzly to configure access.

2. After configuring access, authenticate through Pizzly. A typical OAuth sign-in form will be displayed, confirm the authorization and an `Auth ID` will be generated. *This identifier is a required HTTP header*. The proxy service is now ready to be used.

3. To use the service, make an HTTP request to `https://proxy-pizzly.herokuapp.com/proxy` followed by the path for a particular service that's already been configured, e.g. `/google-drive-upload`. The following path is now dependent on the API endpoint that you're requesting, e.g. `/files`. Finally, include any necessary query params. The complete URL would look something like this: `https://proxy-pizzly.herokuapp.com/proxy/google-drive-upload/files?foo=bar`. The HTTP method is dependent on the destination API endpoint.

4. The request to the proxy is routed to the API endpoint. For example, the previous example would route to `https://www.googleapis.com/upload/drive/v3/files?foo=bar`.

A few things to note:
- Include `Pizzly-Auth-Id` as an HTTP header using the value as described in step two. *This header is mandatory*.
- HTTP headers that are necessary for the destination API endpoint must be prefixed with `Pizzly-Proxy-`. E.g. `Pizzly-Proxy-Content-Type`. Only properly prefixed headers will be routed.
- Include `pizzly_pkey={value}` as a query param. *This param is mandatory*.

## Documentation

- Check out the [application overview](/docs/overview.md).
- For guides, tutorials and references take a look at the [Docs](/docs/readme.md).
