# ACA-Py Development Webhook Server

An easy-to-use webhook server to use for ACA-Py development. It will listen for requests and visualizes them in a dashboard.

Uses [MockServer](https://mock-server.com) under the hood.

## Usage

```sh
docker run -p 1080:1080 ghcr.io/timoglastra/acapy-development-webhook-server
```

Then point the `--webhook-url` to the docker container.

```sh
aries_cloudagent start ... --webhook-url http://localhost:1080
```

### ACA-Py inside docker

If you run ACA-Py in a docker container (e.g. using the `./scripts/run_docker` script in the ACA-Py repo) you need to use the docker host instead of localhost. Beware that you need to use the internal docker port now, so if you changed the `-p 1080:1080` to something else (like `-p 3000:1080`) you still need to use `1080` when running ACA-Py inside docker

```sh
export DOCKERHOST=$(docker run --rm --net=host eclipse/che-ip)

aries_cloudagent start ... --webhook-url http://${DOCKERHOST}:1080
```

### Docker Compose

You can also use docker-compose. This way you don't need to use the `DOCKERHOST` variable. See [./docker-compose.yml](./docker-compose.yml) for a working example.

```yaml
version: "3.5"

services:
  aca-py:
    # Use any version you like
    image: bcgovimages/aries-cloudagent:py36-1.15-0_0.5.6
    ports:
      - "3001:3001"
      - "3000:3000"
    entrypoint: aca-py start ... --webhook-url http://webhook-server:1080

  webhook-server:
    image: ghcr.io/timoglastra/acapy-development-webhook-server
    ports:
      - 1080:1080
```
