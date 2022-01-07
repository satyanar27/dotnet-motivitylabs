FROM bitnami/dotnet-sdk:5
WORKDIR /src
COPY *.csproj .
RUN dotnet restore EmployeeProject.csproj
COPY . .
RUN dotnet build EmployeeProject.csproj --configuration Release -o /app/build
RUN ls -lrth
FROM mcr.microsoft.com/dotnet/aspnet:5.0
WORKDIR /App


