#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.


FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["", "example1/"]
RUN dotnet restore "example1/example1.csproj"
COPY . .
WORKDIR "/src/example1"
RUN dotnet build "example1.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "example1.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "example1.dll"]

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
#COPY ["DemoAPIAzure467/DemoAPIAzure467.csproj", "DemoAzureApi/"]
#COPY /DemoAPIAzure467/DemoAPIAzure467.csproj . 
COPY example1.csproj .
RUN dotnet restore
COPY . .
RUN dotnet publish -c release -o /app
FROM mcr.microsoft.com/dotnet/aspnet:5.0
WORKDIR /app COPY --from=build /app . ENTRYPOINT ["dotnet", "example1.dll"]
