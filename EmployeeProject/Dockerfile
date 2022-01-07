FROM bitnami/dotnet-sdk:5
WORKDIR /src
COPY *.csproj .
RUN ls -lrth
RUN dotnet restore EmployeeProject.csproj
