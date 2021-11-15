#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0-alpine3.13 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-alpine3.13 AS build
WORKDIR /src
COPY ["azure-boards-estimate-backend/Estimate.csproj", "azure-boards-estimate-backend/"]
RUN dotnet restore "azure-boards-estimate-backend/Estimate.csproj"
COPY . .
WORKDIR "/src/azure-boards-estimate-backend"
RUN dotnet build "Estimate.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Estimate.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Estimate.dll"]