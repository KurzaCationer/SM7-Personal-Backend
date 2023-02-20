# syntax=docker/dockerfile:1
FROM mcr.microsoft.com/dotnet/aspnet:7.0-alpine AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0-alpine AS restore
WORKDIR /src
COPY --link ["nuget.cicd.config", "Backend/Backend.csproj", "./"]
RUN dotnet restore --configfile nuget.cicd.config

FROM restore AS build
COPY --link . .
WORKDIR /src/Backend
RUN dotnet build "Backend.csproj" -c Release && \
    dotnet publish "Backend.csproj" --no-restore --no-build -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
COPY --link --from=build /app/publish .
ENTRYPOINT ["dotnet", "Backend.dll"]
