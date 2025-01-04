FROM mcr.microsoft.com/devcontainers/base:ubuntu

# Install dependencies and PostgreSQL client
RUN apt-get update && \
    apt-get install -y wget gnupg lsb-release && \
    # Add PostgreSQL GPG key
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor -o /usr/share/keyrings/pgdg-keyring.gpg && \
    # Configure PostgreSQL repository with signed-by
    echo "deb [signed-by=/usr/share/keyrings/pgdg-keyring.gpg] http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
    apt-get update && \
    apt-get install -y postgresql-client-15 && \
    rm -rf /var/lib/apt/lists/*




# Install uv with proxy settings
RUN http_proxy=http://host.docker.internal:2022 \
https_proxy=http://host.docker.internal:2022 \
all_proxy=socks5://host.docker.internal:1088 \
curl -LsSf https://astral.sh/uv/install.sh | sh

# Add uv to PATH and install Python 3.12
ENV PATH="/root/.local/bin:${PATH}" \
    UV_LINK_MODE=copy
RUN /bin/bash -c "source ~/.local/bin/env && uv python install 3.12"

# Install nvm
RUN http_proxy=http://host.docker.internal:2022 \
    https_proxy=http://host.docker.internal:2022 \
    all_proxy=socks5://host.docker.internal:1088 \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash

# Set up nvm and install Node.js LTS
ENV NVM_DIR=/root/.nvm
RUN . "$NVM_DIR/nvm.sh" && \
    nvm install --lts && \
    nvm use --lts && \
    nvm alias default 'lts/*'

# Add nvm to shell rc files
RUN echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"' >> ~/.bashrc
