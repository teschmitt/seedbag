# seedback / dorfplatz.local

A collection of services for local community communication and collaboration

* a (image) board ([nyx](https://github.com/rls-moe/nyx))
* a collaborative document editor ([etherpad](https://etherpad.org))
* a markdown notes and presentation system ([hedgedoc](https://hedgedoc.org))
* a secure chat system ([ssh-chat](https://github.com/shazow/ssh-chat))

## Building and Running

Just use `docker-compose build` to prepare all containers.

Afterwards, `docker-compose up` starts all services and makes them available via `http://localhost:8000`.

The ssh-based chat is exposed via port `9004`.



## Cross-Platform Building

Follow [this guide](https://medium.com/@artur.klauser/building-multi-architecture-docker-images-with-buildx-27d80f7e2408) to install and enable cross-platform building with `docker buildx`.

Then build all images and upload them to your favorite registry. Not all images can be built for all paltforms, but `arm64` and `amd64` will always work:

```shell
docker buildx build --tag "<docker-id>/seedbag-frontend:latest" --platform linux/arm64,linux/arm/v7,linux/arm/v6,linux/amd64 --push frontend && \
docker buildx build --tag "<docker-id>/seedbag-nyx:latest" --platform linux/arm64,linux/arm/v7,linux/arm/v6,linux/amd64 --push nyx && \
docker buildx build --tag "<docker-id>/seedbag-hedgedoc:latest" --platform linux/arm64,linux/arm/v7,linux/amd64 --push hedgedoc && \
docker buildx build --tag "<docker-id>/seedbag-etherpad:latest" --platform linux/arm64,linux/arm/v7,linux/amd64 --push etherpad && \
docker buildx build --tag "<docker-id>/seedbag-wssh:latest" --platform linux/arm64,linux/amd64 --push wssh && \
docker buildx build --tag "<docker-id>/seedbag-chat:latest" --platform linux/arm64,linux/arm/v7,linux/amd64 --push chat
```

Configure `compose.yml` to pull those images instead of building them on the device.
