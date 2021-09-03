# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.192.0/containers/dotnet/.devcontainer/base.Dockerfile

# [Choice] .NET version: 5.0, 3.1, 2.1
ARG VARIANT="5.0"
FROM mcr.microsoft.com/vscode/devcontainers/dotnet:0-${VARIANT}

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Option] Install Azure CLI
ARG INSTALL_AZURE_CLI="true"
COPY library-scripts/*.sh library-scripts/*.env /tmp/library-scripts/
RUN if [ "$INSTALL_AZURE_CLI" = "true" ]; then bash /tmp/library-scripts/azcli-debian.sh; fi \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends  build-essential cmake libcmocka-dev libcmocka0 gcovr lcov doxygen curl libcurl4-openssl-dev libssl-dev ca-certificates 

RUN git clone https://github.com/Microsoft/vcpkg.git ~/vcpkg && \
    cd ~/vcpkg && \
    git checkout tags/2020.06 && \
    ./bootstrap-vcpkg.sh && \
    ./vcpkg install --triplet x64-linux curl cmocka paho-mqtt && \
    echo 'export VCPKG_DEFAULT_TRIPLET=x64-linux' >> ~/.bashrc && \
    echo 'export VCPKG_ROOT=~/vcpkg' >> ~/.bashrc

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1