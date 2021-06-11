#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["Services/documan.api/documan.api.csproj", "Services/documan.api/"]
COPY ["Services/documan.domain/documan.domain.csproj", "Services/documan.domain/"]
COPY ["Services/documan.infrastructure/documan.infrastructure.csproj", "Services/documan.infrastructure/"]
RUN dotnet restore "Services/documan.api/documan.api.csproj"
COPY . .
WORKDIR "/src/Services/documan.api"
RUN dotnet build "documan.api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "documan.api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "documan.api.dll"]
