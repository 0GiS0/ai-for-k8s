FROM mcr.microsoft.com/devcontainers/base:bullseye

WORKDIR /src
ARG ARCH=amd64


# Install Prerrequisites
RUN if[ "${ARCH}" = "amd64" ]; then \
    apt-get update && \
    apt-get install build-essential procps curl file git --no-install-recommends -y && \
    rm -rf /var/lib/apt/lists/*; \
fi

USER vscode

ENV PATH="/home/linuxbrew/.linuxbrew/bin:${PATH}"

RUN if[ "${ARCH}" = "amd64" ]; then \
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)" && \
    sudo chown -R vscode /home/linuxbrew/.linuxbrew/Cellar \
    && brew tap \
    k8sgpt-ai/k8sgpt \
    && brew tap sozercan/kubectl-ai https://github.com/sozercan/kubectl-ai \
    && brew install \
    k8sgpt \
    kubectl-ai; \
fi