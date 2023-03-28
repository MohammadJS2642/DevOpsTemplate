#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
LABEL org.opencontainers.image.source=https://github.com/MohammadJS2642/MyPackageDevelopment
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src

ENV PASS=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
ENV USERNAME=MohammadJS2642
ENV URL="https://nuget.pkg.github.com/MohammadJS2642/index.json"

#COPY ["nuget.config", "."]
COPY ["packages.lock.json", "WebApplication2/"]
RUN dotnet nuget add source --username $USERNAME --store-password-in-clear-text -p $PASS  --name github $URL
RUN dotnet nuget list source
COPY ["WebApplication2.csproj", "WebApplication2/"]
RUN dotnet restore "WebApplication2/WebApplication2.csproj"  --locked-mode -s $URL
COPY . .
WORKDIR "/src/WebApplication2"
RUN dotnet build "WebApplication2.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApplication2.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApplication2.dll"]