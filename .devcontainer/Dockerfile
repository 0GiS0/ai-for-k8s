FROM mcr.microsoft.com/devcontainers/base:bullseye

WORKDIR /src

# Install Prerrequisites
RUN apt-get update && \
    apt-get install build-essential procps curl file git --no-install-recommends -y && \
    rm -rf /var/lib/apt/lists/*

USER vscode
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)" && \
    sudo chown -R vscode /home/linuxbrew/.linuxbrew/Cellar

# USER vscode
ENV PATH="/home/linuxbrew/.linuxbrew/bin:${PATH}"

# Tap repositories
RUN brew tap \
k8sgpt-ai/k8sgpt \
&& brew tap sozercan/kubectl-ai https://github.com/sozercan/kubectl-ai

# Install tools
RUN brew install \
k8sgpt \
kubectl-ai