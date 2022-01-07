FROM bitnami/dotnet-sdk
  #RUN pwd && ls -l && rm -rf ./* && ls -l
  #COPY ./EmployeeProject.csproj ./
  COPY */*.csproj ./
  RUN for file in $(ls *.csproj); do mkdir -p ./${file%.*}/ && mv $file ./${file%.*}/; done
  RUN ls -l 
  #RUN dotnet restore EmployeeProject.csproj --no-cache && dotnet build EmployeeProject.csproj --configuration Release  
  RUN dotnet restore EmployeeProject.csproj
  COPY . .
  RUN dotnet build EmployeeProject.csproj --configuration Release

