FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
WORKDIR /app

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["PizzaConsoleApp/PizzaConsoleApp.csproj", "PizzaConsoleApp/"]
RUN dotnet restore "PizzaConsoleApp/PizzaConsoleApp.csproj"
COPY . .
WORKDIR "/src/PizzaConsoleApp"
RUN dotnet build "PizzaConsoleApp.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "PizzaConsoleApp.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PizzaConsoleApp.dll"]
