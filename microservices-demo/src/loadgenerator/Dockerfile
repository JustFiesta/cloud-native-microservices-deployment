FROM --platform=$BUILDPLATFORM python:3.12.11-alpine3.22@sha256:02a73ead8397e904cea6d17e18516f1df3590e05dc8823bd5b1c7f849227d272 AS build

ARG TARGETOS=linux
ARG TARGETARCH=amd64

WORKDIR /app

RUN apk add --no-cache gcc musl-dev linux-headers

COPY requirements.txt .
RUN pip wheel --no-cache-dir --wheel-dir /wheels -r requirements.txt

COPY . .

FROM python:3.12.11-alpine3.22@sha256:02a73ead8397e904cea6d17e18516f1df3590e05dc8823bd5b1c7f849227d272 AS runtime

WORKDIR /app

COPY --chown=1000:1000 --from=build /wheels /wheels
RUN pip install --no-cache-dir /wheels/*

COPY --chown=1000:1000 locustfile.py .

USER 1000:1000

ENTRYPOINT [ "locust" ]
CMD ["-f", "locustfile.py", "--host", "http://${FRONTEND_ADDR}", "--headless", "-u", "${USERS:-10}", "-r", "${RATE:-1}", "2>&1"]