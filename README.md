# Authelia OIDC multi-domain

An example of using an OAuth/OIDC client to authenticate a secondary root domain.

- Requires `docker` and `docker-compose`.
- Requires your local DNS resolver to support `*.localhost` subdomains.
- Assumes the `traefik` network name is not yet being used.
- Assumes your `docker0` bridge network uses `172.17.0.1/16`.
  (If not, update `extra_hosts` for the `second-auth` service accordingly.)

## Howto

```sh
docker-compose up
```

This will take some time to settle and come online.

1. With http://localhost:8080 you can go directly to Traefik's dashboard to watch which containers are ready.
1. When all routes turn green, you can go to https://whoami.second.localhost (will show UNKNOWN ISSUER warning in the browser).
1. This redirects to https://authelia.first.localhost (again UNKNOWN ISSUER), login is `authelia:authelia`.
1. An OIDC consent page for "*.second.localhost Forward Auth container" should appear.
1. After accepting redirects to https://auth.second.localhost/_oauth (again UNKNOWN ISSUER).
1. Finally landing on https://whoami.second.localhost, where you should see `X-Forwarded-User: authelia@first.localhost`

## Discussion

This approach has a couple of downsides, you can find and join discussions here:
https://github.com/authelia/authelia/issues/1198#issuecomment-1082994968
