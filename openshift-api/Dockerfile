FROM mcr.microsoft.com/dotnet/aspnet:5.0-focal AS base
WORKDIR /app
EXPOSE 8080

ENV ASPNETCORE_URLS=http://*:8080

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" docker && chown -R docker /app
USER docker

FROM mcr.microsoft.com/dotnet/sdk:5.0-focal AS build
WORKDIR /src
COPY ["openshift-api.csproj", "./"]
RUN dotnet restore "openshift-api.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "openshift-api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "openshift-api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "openshift-api.dll"]
