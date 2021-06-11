#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:5.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["Services/documan.worker/documan.worker.csproj", "Services/documan.worker/"]
COPY ["Services/documan.domain/documan.domain.csproj", "Services/documan.domain/"]
COPY ["Services/documan.infrastructure/documan.infrastructure.csproj", "Services/documan.infrastructure/"]
RUN dotnet restore "Services/documan.worker/documan.worker.csproj"
COPY . .
WORKDIR "/src/Services/documan.worker"
RUN dotnet build "documan.worker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "documan.worker.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "documan.worker.dll"]
