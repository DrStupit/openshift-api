#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.
#
#FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
#WORKDIR /app
#EXPOSE 80
#EXPOSE 443
#
#FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
#WORKDIR /src
#COPY ["openshift-api/openshift-api.csproj", "openshift-api/"]
#RUN dotnet restore "openshift-api/openshift-api.csproj"
#COPY . .
#WORKDIR "/src/openshift-api"
#RUN dotnet build "openshift-api.csproj" -c Release -o /app/build
#
#FROM build AS publish
#RUN dotnet publish "openshift-api.csproj" -c Release -o /app/publish
#
#FROM base AS final
#WORKDIR /app
#COPY --from=publish /app/publish .
#ENTRYPOINT ["dotnet", "openshift-api.dll"]


#FROM mcr.microsoft.com/dotnet/aspnet:5.0
#
#COPY bin/Release/net5.0/publish/ App/
#
#WORKDIR /App
#
#ENTRYPOINT ["dotnet", "openshift-api.dll"]

# syntax=docker/dockerfile:1
FROM FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY ../engine/examples ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:3.1
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "openshift-api.dll"]