FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ByteNow.Browsewarden.Api/ByteNow.Browsewarden.Api.csproj", "ByteNow.Browsewarden.Api/"]
RUN dotnet restore "ByteNow.Browsewarden.Api/ByteNow.Browsewarden.Api.csproj"
COPY . .
WORKDIR "/src/ByteNow.Browsewarden.Api"
RUN dotnet build "ByteNow.Browsewarden.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ByteNow.Browsewarden.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ByteNow.Browsewarden.Api.dll"]
