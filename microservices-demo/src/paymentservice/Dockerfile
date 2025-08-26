FROM --platform=$BUILDPLATFORM node:16.20.2@sha256:f77a1aef2da8d83e45ec990f45df50f1a286c5fe8bbfb8c6e4246c6389705c0b AS build

ARG TARGETOS=linux
ARG TARGETARCH=x64

WORKDIR /app
COPY package*.json ./

RUN npm ci --only=production

COPY server.js ./
COPY proto ./proto

FROM gcr.io/distroless/nodejs16-debian11 AS runtime

WORKDIR /app
COPY --chown=1000:1000 --from=build /app/node_modules ./node_modules
COPY --chown=1000:1000 . .

ENV PORT=50051

EXPOSE 50051
USER 1000:1000

CMD ["index.js"]