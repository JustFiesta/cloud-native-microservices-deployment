FROM --platform=$BUILDPLATFORM golang:tip-trixie@sha256:adc08dd4cdb8e208743beb974a5ccf57ec365802bbd4ecee64ec61e1409d83f8 AS build

ARG TARGETOS
ARG TARGETARCH

WORKDIR /app
COPY . .

RUN go mod download

RUN CGO_ENABLED=0 GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -o /app/frontend

FROM scratch AS runtime

COPY --chown=1000:1000 --from=build /app/frontend /
COPY --chown=1000:1000 templates /templates
COPY --chown=1000:1000 static /static

EXPOSE 8080
USER 1000:1000

ENTRYPOINT [ "/frontend" ]