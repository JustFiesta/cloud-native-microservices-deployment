FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build

WORKDIR /App
COPY . .

RUN dotnet restore ./cartservice.sln
RUN dotnet publish -o out

FROM mcr.microsoft.com/dotnet/aspnet:9.0-alpine AS runtime

WORKDIR /App
COPY --from=build /App/out .

RUN ls -la > /App/files.txt

ENTRYPOINT ["dotnet", "cartservice.dll"]