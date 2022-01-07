FROM bitnami/dotnet-sdk:5 AS build
WORKDIR /src
COPY *.csproj .
RUN dotnet restore EmployeeProject.csproj
COPY . .
RUN dotnet build EmployeeProject.csproj --configuration Release -o /app/build
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS publish
WORKDIR /app
COPY --from=build /app/build .
RUN ls -lrth

