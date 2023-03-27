#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /root
ENV PASS=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
ENV USERNAME=MohammadJS2642
ENV URL="https://nuget.pkg.github.com/MohammadJS2642/index.json"
COPY ["WebApplication2.csproj", "WebApplication2/"]
RUN ["mkdir",  "-p", "root/.nuget/NuGet"]
#RUN ["touch", ".nuget/NuGet/NuGet.config"]
RUN dotnet nuget add source --username $USERNAME --store-password-in-clear-text --configfile ".nuget/NuGet/NuGet.config" -p $PASS --name github $URL

WORKDIR /src
#COPY ["nuget.config", "."]
#RUN ["cp", "nuget.config", "root/.nuget/NuGet/NuGet.config"]
#RUN ["cat","root/.nuget/NuGet/NuGet.config"]
RUN dotnet restore "WebApplication2/WebApplication2.csproj" --configfile "/root/.nuget/NuGet/NuGet.config"
COPY . .
WORKDIR "/src/WebApplication2"
RUN dotnet build "WebApplication2.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApplication2.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApplication2.dll"]