#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["TestDotnet7/TestDotnet7.csproj", "TestDotnet7/"]
RUN dotnet restore "TestDotnet7/TestDotnet7.csproj"
COPY . .
WORKDIR "/src/TestDotnet7"
RUN dotnet build "TestDotnet7.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TestDotnet7.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TestDotnet7.dll"]