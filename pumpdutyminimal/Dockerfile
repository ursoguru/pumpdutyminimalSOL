#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
#COPY ["pumpdutyminimal/wwwroot/*.xml", "pumpdutyminimal/wwwroot/"]
COPY ["pumpdutyminimal/pumpdutyminimal.csproj", "pumpdutyminimal/"]
RUN dotnet restore "pumpdutyminimal/pumpdutyminimal.csproj"
COPY . .
WORKDIR "/src/pumpdutyminimal"
RUN dotnet build "pumpdutyminimal.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "pumpdutyminimal.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /src
COPY ["pumpdutyminimal/wwwroot/*.xml", "pumpdutyminimal/wwwroot/"]
WORKDIR /app
COPY --from=publish /app/publish .
#COPY /app/wwwroot/*.xml wwwroot
ENTRYPOINT ["dotnet", "pumpdutyminimal.dll"]
