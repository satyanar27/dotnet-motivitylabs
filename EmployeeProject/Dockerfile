FROM bitnami/dotnet-sdk:5
WORKDIR /src
COPY *.csproj .
RUN dotnet restore EmployeeProject.csproj
COPY . .
RUN ls -lrth
RUN dotnet build EmployeeProject.csproj --configuration Release
