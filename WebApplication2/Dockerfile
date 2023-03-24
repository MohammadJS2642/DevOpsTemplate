#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 8000
EXPOSE 4430

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ENV PASS=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
ENV USERNAME=MohammadJS2642
ENV URL="https://nuget.pkg.github.com/MohammadJS2642/index.json"
WORKDIR /src
COPY ["WebApplication2.csproj", "WebApplication2/"]
COPY . WebApplication2
COPY ["WebApplication2/nuget.config.template", "WebApplication2/nuget.config"]
RUN ["cat", "WebApplication2/nuget.config"]
#RUN ["ls", "/root/.nuget/NuGet/NuGet.Config"]
#RUN ["cp", "-f", "WebApplication2/nuget.config", "/root/.nuget/NuGet/NuGet.Config"]
RUN dotnet nuget add source --username $USERNAME --store-password-in-clear-text --configfile "WebApplication2/nuget.config" -p $PASS  --name github $URL
RUN dotnet nuget list source
RUN dotnet restore --configfile "WebApplication2/nuget.config" "WebApplication2/WebApplication2.csproj"
WORKDIR "/src/WebApplication2"
RUN dotnet build "WebApplication2.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApplication2.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApplication2.dll"]