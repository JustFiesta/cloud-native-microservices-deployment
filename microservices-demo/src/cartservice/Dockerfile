FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build

# Default target architecture
# This can be overridden at build time with --build-arg TARGETARCH=linux-x64
ARG TARGETARCH=linux-x64

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y gcc

WORKDIR /App
COPY . .

RUN dotnet restore ./cartservice.sln \
    -r ${TARGETARCH}

RUN dotnet publish "src/cartservice.csproj" \
    -c Release \
    -o out \
    -r ${TARGETARCH} \
    --self-contained true \
    --no-restore \
    /p:PublishTrimmed=true \
    /p:PublishSingleFile=true\ 
    /p:DebugType=None \
    /p:DebugSymbols=false

FROM gcr.io/distroless/cc-debian12 AS runtime

WORKDIR /App
COPY --from=build /App/out .

ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1

ENTRYPOINT ["/App/cartservice"]