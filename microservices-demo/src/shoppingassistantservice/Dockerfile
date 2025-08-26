FROM --platform=$BUILDPLATFORM python:3.12.11-slim@sha256:d67a7b66b989ad6b6d6b10d428dcc5e0bfc3e5f88906e67d490c4d3daac57047 AS build

ARG TARGETOS=linux
ARG TARGETARCH=amd64

WORKDIR /app

RUN apt-get update \
    && apt-get upgrade \
    && apt-get install -y g++

COPY requirements.txt .
RUN pip wheel --no-cache-dir --wheel-dir /wheels -r requirements.txt

COPY . .

FROM python:3.12.11-slim@sha256:d67a7b66b989ad6b6d6b10d428dcc5e0bfc3e5f88906e67d490c4d3daac57047 AS runtime

WORKDIR /app

COPY --chown=1000:1000 --from=build /wheels /wheels
RUN pip install --no-cache-dir /wheels/*

COPY --chown=1000:1000 --from=build /app /app

ENV PORT=8080

EXPOSE 8080
USER 1000:1000

ENTRYPOINT [ "python", "shoppingassistantservice.py" ]