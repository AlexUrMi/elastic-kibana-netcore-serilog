#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:2.1-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch AS build
WORKDIR /src
COPY ["Elastic.Serilog.Web/Elastic.Serilog.Web.csproj", "Elastic.Serilog.Web/"]
RUN dotnet restore "Elastic.Serilog.Web/Elastic.Serilog.Web.csproj"
COPY . .
WORKDIR "/src/Elastic.Serilog.Web"
RUN dotnet build "Elastic.Serilog.Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Elastic.Serilog.Web.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Elastic.Serilog.Web.dll"]