#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build

RUN apt update && apt upgrade
RUN apt install ghostscript
WORKDIR /src
COPY ["TownSuite.ConversionServer.APISite/TownSuite.ConversionServer.APISite.csproj", "TownSuite.ConversionServer.APISite/"]
RUN dotnet restore "TownSuite.ConversionServer.APISite/TownSuite.ConversionServer.APISite.csproj"
COPY . .
WORKDIR "/src/TownSuite.ConversionServer.APISite"
RUN dotnet build "TownSuite.ConversionServer.APISite.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TownSuite.ConversionServer.APISite.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TownSuite.ConversionServer.APISite.dll"]