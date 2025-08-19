FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:9.0 AS build

# Default target architecture
# This can be overridden at build time with --platform flag 
ARG TARGETOS=linux
ARG TARGETARCH=x64
ARG TARGETRID=${TARGETOS}-${TARGETARCH}
ARG TARGETPLATFORM

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y gcc

WORKDIR /App
COPY . .

RUN dotnet restore ./cartservice.sln \
    -r ${TARGETRID}

RUN dotnet publish "src/cartservice.csproj" \
    -c Release \
    -o out \
    -r ${TARGETRID} \
    --self-contained true \
    --no-restore \
    /p:PublishTrimmed=true \
    /p:PublishSingleFile=true\ 
    /p:DebugType=None \
    /p:DebugSymbols=false

FROM gcr.io/distroless/cc-debian12 AS runtime

WORKDIR /App
COPY --chown=1000:1000 --from=build /App/out .

ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1 \
    DOTNET_EnableDiagnostics=0 \
    ASPNETCORE_HTTP_PORTS=7070

EXPOSE 7070 
USER 1000:1000

ENTRYPOINT ["/App/cartservice"]