# .devcontainer/Dockerfile
FROM python:3.11-slim

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# Set up basic tools
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    zsh \
    fzf \
    && rm -rf /var/lib/apt/lists/*

# Manually download fzf key bindings
RUN mkdir -p /usr/share/fzf \
    && wget https://raw.githubusercontent.com/junegunn/fzf/master/shell/key-bindings.zsh -O /usr/share/doc/fzf/examples/key-bindings.zsh

# Create a non-root user
ARG USERNAME="vscode"
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo "$USERNAME ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Set the user
USER $USERNAME

# Install Oh My Zsh
RUN sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)" "" --unattended

# Download your custom .zshrc from GitHub
RUN curl -o /home/$USERNAME/.zshrc https://raw.githubusercontent.com/SniperSlayerss/dotfiles/master/common/.zshrc \
            && chown $USERNAME:$USERNAME /home/$USERNAME/.zshrc
 
# Set the working directory
WORKDIR /workspace

# Set the default shell to Zsh
CMD [ "zsh" ]