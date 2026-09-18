# syntax=docker/dockerfile:1

FROM golang:alpine@sha256:4cb7ac979db5fcc41cae44b2227ba5ab8a51e8807f40d9ba4dee20a0ad960b5b AS base
ENV CGO_ENABLED=0
RUN apk add --no-cache file git
WORKDIR /src

FROM base AS build
RUN --mount=type=bind,target=/src \
    --mount=type=cache,target=/root/.cache/go-build \
    go build -ldflags "-s -w" -o /usr/bin/app .

FROM scratch AS binary
COPY --from=build /usr/bin/app /bin/app

FROM alpine AS image
COPY --from=build /usr/bin/app /bin/app
EXPOSE 8080
ENTRYPOINT ["/bin/app"]
