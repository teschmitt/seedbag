FROM golang:1.18-buster AS build-env

# until this gets merged:
# https://github.com/rls-moe/nyx/pull/13
# this must stay commented:
# RUN cd /root && git clone https://github.com/rls-moe/nyx
# and we'll be pulling the modified version from a fork:
RUN cd /root && \
  git clone https://github.com/teschmitt/nyx.git

WORKDIR /root/nyx

RUN go build -o $GOPATH/bin/nyx

FROM debian:bullseye

EXPOSE 8080

RUN groupadd \
  --gid 1000 \
  nyx && \
  useradd \
  --home-dir /opt/nyx \
  --comment "Nyx Board" \
  --gid nyx \
  --create-home \
  --no-user-group \
  --uid 1000 \
  --shell /bin/bash \
  nyx

COPY --from=build-env /go/bin/nyx /opt/nyx/nyx
COPY --from=build-env /root/nyx/config.example.yml /opt/nyx/config.yml
COPY config.yml /opt/nyx/config.yml
RUN chown -R \
  nyx:nyx /opt/nyx

WORKDIR /opt/nyx
USER nyx
CMD [ "/opt/nyx/nyx" ]