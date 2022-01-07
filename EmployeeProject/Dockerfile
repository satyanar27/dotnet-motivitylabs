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
ENTRYPOINT ["dotnet", "EmployeeProject.dll"]
RUN curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
FROM python:3.7-alpine  AS deploy
COPY ./ /usr/src/app/
WORKDIR /usr/src/app
RUN apk add curl openssl bash --no-cache
RUN curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl" \
    && chmod +x ./kubectl \
    && mv ./kubectl /usr/local/bin/kubectl \
    && curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 \
    && chmod +x get_helm.sh && ./get_helm.sh
