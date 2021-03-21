# Event Stream API

This is a RESTful API for Event Streams with rich user event notification integration support.

# Installation

Use Docker. Until v1 is published you will need to build it locally and push the image to a registry.

TODO: Publish image at v1.0.0...

The Dockerfile in the project root is meant for prod.

TODO: helm?

## Dependencies

### Database

The Event Stream API requires a database. Migrations are provided for a Postgres DB, but the system is API agnostic.

#### Running migrations

Run this command to trigger migrations (when the api is running as a local docker):

```docker exec -it eventstreamapi bin/console doctrine:migrations:migrate```

### Transports Queue

To leverage transports, a queue service is required. Notification events will be published to the queue configured by 
the `MESSENGER_TRANSPORT_DSN` env var. Supported transports are... TODO


## Environmental Variables

Variable | Purpose | Example
--- | --- | ---
`DATABASE_URL`      | This should be set to the connection uri (DSN) to the DB. | `postgresql://user:password@hostname:5432/dbname?serverVersion=11&charset=utf8`
`JWKS_URI`          | This should be set to the URI to fetch the JWK set from. | `https://postchat.us.auth0.com/.well-known/jwks.json`
`JWT_ISSUER`        | This should be set to the issuer string that should be trusted in signed JWTs. | `https://postchat.us.auth0.com/`
`JWT_AUDIENCE`      | This should be set to the audience that represents this API. Tokens without this audience will be rejected. | `https://api.getpostchat.com/`
`CORS_ALLOW_ORIGIN` | This is the origins allowed for CORS. It is a regex string. | `^https?://(localhost\|127\.0\.0\.1)(:[0-9]+)?$`
`MESSENGER_TRANSPORT_DSN` | This configures the transport for the notification events that subscriptions generate to transport handlers. | `sync://`
`MESSENGER_RETURN_TRANSPORT_DSN` | This configures the transport for the return events that transports can generate. | `sync://`

# Usage

See the (/docs/)[/docs/] folder for some guides on how various parts of the API function. 

## SDKs

## API Documentation

# TODO
Messages sent to transports for a subscription should generate an entry on that fact somewhere (sub log?) This can be added to / updated by the transport?
Transports should be able to send data back without credentials/api calls. Relevant to ^ because responses for a sub log can come from that.

Publish webhook transport from postchat
Publish simplified typescipt sdk

Add messaging lib back in (enqueue) now that it supports php 8
Helm chart to deploy to kube with a DB + messaging backend? (package a messaging backend somehow in helm)

Sample project that uses the API as part of a larger project. Webchat?
Health care messaging. Immutability + really easy with api design to be hippa compliant (read logs is all that are missing, and are easy)

Add event modifier field ("parent" reference to another event) to event.