FROM --platform=$BUILDPLATFORM python:3.12.11-slim@sha256:d67a7b66b989ad6b6d6b10d428dcc5e0bfc3e5f88906e67d490c4d3daac57047 AS build

ARG TARGETOS=linux
ARG TARGETARCH=amd64

WORKDIR /app

RUN apt-get install -y --no-install-recommends g++

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

FROM python:3.12.11-slim@sha256:d67a7b66b989ad6b6d6b10d428dcc5e0bfc3e5f88906e67d490c4d3daac57047 AS runtime

WORKDIR /app

COPY --chown=1000:1000 --from=build /usr/local/lib/python3.12 /usr/local/lib/python3.12/
COPY --chown=1000:1000 --from=build /app /app

ENV PORT=8080

EXPOSE 8080
USER 1000:1000

ENTRYPOINT [ "python", "shoppingassistantservice.py" ]