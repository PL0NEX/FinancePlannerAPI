FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["financeplannerapi/financeplannerapi.csproj", "financeplannerapi/"]
RUN dotnet restore "./financeplannerapi/financeplannerapi.csproj"
COPY . .
WORKDIR "/src/financeplannerapi"
RUN dotnet build "./financeplannerapi.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./financeplannerapi.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "financeplannerapi.dll"]
