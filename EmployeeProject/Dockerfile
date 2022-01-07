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
FROM alpine:3.14
ENV VERSION v3.6.3
WORKDIR /
RUN apk --update add ca-certificates wget python3 curl tar jq
ENV HOME /
ENV PATH /google-cloud-sdk/bin:$PATH
ENV CLOUDSDK_PYTHON_SITEPACKAGES 1
RUN wget https://dl.google.com/dl/cloudsdk/channels/rapid/google-cloud-sdk.zip && unzip google-cloud-sdk.zip && rm google-cloud-sdk.zip
RUN google-cloud-sdk/install.sh --usage-reporting=true --path-update=true --bash-completion=true --rc-path=/.bashrc --additional-components app kubectl alpha beta
RUN google-cloud-sdk/bin/gcloud config set --installation component_manager/disable_update_check true
ENV FILENAME helm-${VERSION}-linux-amd64.tar.gz
ENV HELM_URL https://get.helm.sh/${FILENAME}
RUN echo $HELM_URL
RUN curl -o /tmp/$FILENAME ${HELM_URL} \
  && tar -zxvf /tmp/${FILENAME} -C /tmp \
  && mv /tmp/linux-amd64/helm /bin/helm \
  && rm -rf /tmp
RUN apk --update add git bash
ENV BUILD_DEPS="gettext"  \
    RUNTIME_DEPS="libintl"
RUN set -x && \
    apk add --update $RUNTIME_DEPS && \
    apk add --virtual build_deps $BUILD_DEPS &&  \
    cp /usr/bin/envsubst /usr/local/bin/envsubst && \
    apk del build_deps
ENV HELM_PLUGIN_DIR /.helm/plugins/helm-diff
RUN mkdir /tmp
RUN helm plugin install https://github.com/viglesiasce/helm-gcs.git
RUN helm plugin install https://github.com/databus23/helm-diff
RUN helm plugin install https://github.com/helm/helm-2to3
ENV VALS_VERSION=0.14.0
ENV VALS_URL=https://github.com/variantdev/vals/releases/download/v${VALS_VERSION}/vals_${VALS_VERSION}_linux_amd64.tar.gz
RUN curl -sL ${VALS_URL} | tar zx -C /usr/local/bin vals
