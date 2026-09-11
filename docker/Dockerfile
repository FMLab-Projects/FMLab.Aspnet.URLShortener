# API - Layered architecture boilerplate
# Copyright (c) 2026 Fagner Marinho
# Licensed under the MIT License. See LICENSE file in the project root for details.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
# curl is only here for the container HEALTHCHECK below — the aspnet image ships
# neither curl nor wget.
RUN apt-get update \
 && apt-get install -y --no-install-recommends curl \
 && rm -rf /var/lib/apt/lists/*
USER $APP_UID
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

COPY ["src/FMLab.Aspnet.URLShortener.Api", "FMLab.Aspnet.URLShortener.Api"]
COPY ["src/FMLab.Aspnet.URLShortener.Business", "FMLab.Aspnet.URLShortener.Business"]
COPY ["src/FMLab.Aspnet.URLShortener.Infrastructure", "FMLab.Aspnet.URLShortener.Infrastructure"]
COPY ["/Directory.Packages.props", "/"]

RUN dotnet restore "FMLab.Aspnet.URLShortener.Api/FMLab.Aspnet.URLShortener.Api.csproj"

COPY src/ /
WORKDIR "/FMLab.Aspnet.URLShortener.Api"
RUN dotnet build "FMLab.Aspnet.URLShortener.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

 FROM build AS publish
 ARG BUILD_CONFIGURATION=Release
 RUN dotnet publish "/FMLab.Aspnet.URLShortener.Api/FMLab.Aspnet.URLShortener.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

 FROM base AS final
 WORKDIR /app
 COPY --from=publish /app/publish .
 HEALTHCHECK --interval=30s --timeout=5s --start-period=20s --retries=3 \
   CMD curl -fsS http://127.0.0.1:8080/api/health || exit 1
 ENTRYPOINT ["dotnet", "FMLab.Aspnet.URLShortener.Api.dll"]
