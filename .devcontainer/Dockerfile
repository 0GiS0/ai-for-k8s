FROM mcr.microsoft.com/devcontainers/base:bullseye

WORKDIR /src

# Install Prerrequisites
RUN apt-get update && \
    apt-get install build-essential procps curl file git --no-install-recommends -y && \
    rm -rf /var/lib/apt/lists/*

# RUN mkdir -p models \
# # Download luna-ai-llama2 to models/
# && wget https://huggingface.co/TheBloke/Luna-AI-Llama2-Uncensored-GGUF/resolve/main/luna-ai-llama2-uncensored.Q4_0.gguf -O models/luna-ai-llama2 \
# # Use a template from the examples
# && cp -rf prompt-templates/getting_started.tmpl models/luna-ai-llama2.tmpl

USER vscode
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)" && \
    sudo chown -R vscode /home/linuxbrew/.linuxbrew/Cellar

ENV PATH="/home/linuxbrew/.linuxbrew/bin:${PATH}"

# Tap repositories
RUN brew tap \
k8sgpt-ai/k8sgpt \
&& brew tap sozercan/kubectl-ai https://github.com/sozercan/kubectl-ai

# Install tools
RUN brew install \
k8sgpt \
kubectl-ai