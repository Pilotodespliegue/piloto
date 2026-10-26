FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 5120

ENV ASPNETCORE_URLS=http://+:5120
ENV TZ="America/Costa_Rica"

RUN apt-get update 
&& apt-get install -y --no-install-recommends tzdata 
&& rm -rf /var/lib/apt/lists/*

USER app

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG configuration=Release
WORKDIR /src

COPY ["TemperaturasApi.csproj", "./"]

RUN dotnet restore "TemperaturasApi.csproj"

COPY . .

WORKDIR "/src/."

RUN dotnet build "TemperaturasApi.csproj" 
-c $configuration 
-o /app/build

FROM build AS publish
ARG configuration=Release

RUN dotnet publish "TemperaturasApi.csproj" 
-c $configuration 
-o /app/publish 
/p:UseAppHost=false

FROM base AS final
WORKDIR /app

COPY --from=publish /app/publish .

ENTRYPOINT ["dotnet", "TemperaturasApi.dll"]
