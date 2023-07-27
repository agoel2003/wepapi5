#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["wepapi5/wepapi5.csproj", "wepapi5/"]
RUN dotnet restore "wepapi5/wepapi5.csproj"
COPY . .
WORKDIR "/src/wepapi5"
RUN dotnet build "wepapi5.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "wepapi5.csproj" -c Release -o /app/publish -r linux-musl-x64 --self-contained false --no-restore

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WepApi5.dll"]
