#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

#RUN --mount=type=secret,id=github_token \
  #cat /run/secrets/github_token
#

WORKDIR /src

ARG NUGETCONFIGFILE

ENV PASS=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
ENV USERNAME=MohammadJS2642
ENV URL="https://nuget.pkg.github.com/MohammadJS2642/index.json"

COPY ["nuget.config", "."]
COPY ["WebApplication1_Solution.sln", "."]
COPY ["packages.lock.json", "WebApplication2/"]
#RUN dotnet nuget locals all --clear
#RUN dotnet nuget add source --username $USERNAME -p $PASS --store-password-in-clear-text --name github $URL
RUN dotnet nuget list source
COPY ["WebApplication2.csproj", "WebApplication2/"]
#RUN dotnet restore "WebApplication2/WebApplication2.csproj" --configfile "WebApplication2/nuget.config"
RUN dotnet restore "WebApplication2/WebApplication2.csproj" --lock-file-path "WebApplication2/packages.lock.json"
COPY . .

WORKDIR "/src/WebApplication2"
RUN dotnet build "WebApplication2.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApplication2.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApplication2.dll"]