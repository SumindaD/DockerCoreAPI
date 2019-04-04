FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY ["NetCore/NetCore.csproj", "NetCore/"]
RUN dotnet restore "NetCore/NetCore.csproj"
COPY . .
WORKDIR "/src/NetCore"
RUN dotnet build "NetCore.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "NetCore.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "NetCore.dll"]

COPY NetCore/mycert.cert /etc/ssl/certs
RUN update-ca-certificates