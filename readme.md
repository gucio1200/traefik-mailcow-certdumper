# Traefik Mailcow Certdumper ![GitHub Workflow Status](https://img.shields.io/github/workflow/status/henkelmax/traefik-mailcow-certdumper/Build) ![GitHub issues](https://img.shields.io/github/issues-raw/henkelmax/traefik-mailcow-certdumper) ![GitHub release (latest by date)](https://img.shields.io/github/v/release/henkelmax/traefik-mailcow-certdumper?include_prereleases)

Dumps certificates from [Traefik](https://containo.us/traefik/) to use them in [Mailcow Dockerized](https://github.com/mailcow/mailcow-dockerized).

## Environment Variables

| Variable       | Default     | Description                                                        |
| -------------- | ----------- | ------------------------------------------------------------------ |
| ACME_JSON_PATH | ./acme.json | The path to the `acme.json` created by Traefik                     |
| DOMAIN         | example.com | The domain to get the certificate from                             |
| CERT_PATH      | ./cert.pem  | The path to write the Certificate to                               |
| KEY_PATH       | ./key.pem   | The path to write the Certificate private key to                  |
| RUN_AT_START   | true        | If you want to dump the certificates at the start of the container |

## Example Stack YML

``` yml
version: "3.3"
services:
  certdumper:
    image: 'docker.pkg.github.com/henkelmax/traefik-mailcow-certdumper/traefik-mailcow-certdumper:latest'
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock" # Used to restart the mailcow services after dumping
      - "/path/to/acme.json:/acme.json:ro" # The path to the acme.json
      - "/path/to/mailcow-dockerized/data/assets/ssl:/ssl" # The path to the Mailcow ssl directory
    environment:
      ACME_JSON_PATH: "/acme.json"
      DOMAIN: "example.com"
      CERT_PATH: "/ssl/cert.pem"
      KEY_PATH: "/ssl/key.pem"
      RUN_AT_START: "true"
```
