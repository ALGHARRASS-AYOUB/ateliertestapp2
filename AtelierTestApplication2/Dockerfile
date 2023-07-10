#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["AtelierTestApplication2.csproj", "AtelierTestApplication2/"]
RUN dotnet restore "AtelierTestApplication2/AtelierTestApplication2.csproj"
COPY . .
WORKDIR "/src/AtelierTestApplication2"
RUN dotnet build "AtelierTestApplication2.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AtelierTestApplication2.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS sdk
WORKDIR /sdk

FROM base AS final
WORKDIR /app
COPY --from=sdk /sdk /usr/share/dotnet
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AtelierTestApplication2.dll"]
