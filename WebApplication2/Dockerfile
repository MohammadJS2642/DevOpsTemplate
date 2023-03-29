#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
LABEL org.opencontainers.image.source=https://github.com/MohammadJS2642/MyPackageDevelopment
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src

ARG PASS
#ENV PASS=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
ENV USERNAME=MohammadJS2642
ENV URL="https://nuget.pkg.github.com/MohammadJS2642/index.json"

#COPY ["nuget.config", "."]
#COPY ["packages.lock.json", "WebApplication2/"]
RUN dotnet nuget locals all --clear
RUN dotnet nuget add source --username $USERNAME -p $PASS --store-password-in-clear-text --name github $URL
RUN dotnet nuget list source
COPY ["WebApplication2.csproj", "WebApplication2/"]

COPY . .

WORKDIR "/src/WebApplication2"
RUN dotnet restore 
RUN dotnet build "WebApplication2.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApplication2.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApplication2.dll"]