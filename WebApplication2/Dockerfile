#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

WORKDIR /src

#ARG pass

COPY "WebApplication1_Solution.sln" "WebApplication1_Solution.sln"
#COPY ["nuget.config", "nuget.config"]

#COPY ["nuget.config", "WebApplication2/"]
#COPY ["packages.lock.json", "WebApplication2/"]
COPY ["WebApplication2/WebApplication2.csproj", "WebApplication2/"]
#RUN dotnet restore "WebApplication2/WebApplication2.csproj" --configfile "./nuget.config"
COPY . .
RUN ["rm", "./nuget.config"]
COPY "./WebApplication2/" "./WebApplication2/"
#RUN dotnet new nugetconfig --force
#RUN dotnet nuget add source --username $USERNAME --store-password-in-clear-text -p $pass  --name github "https://nuget.pkg.github.com/mohammadjs2642/index.json"
RUN dotnet restore ".\WebApplication1_Solution.sln"

WORKDIR "/src/WebApplication2"
RUN dotnet build "WebApplication2.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApplication2.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApplication2.dll"]