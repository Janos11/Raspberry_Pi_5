# Use an ARM-compatible base image (Debian)
FROM arm64v8/debian:bullseye-slim

# Set the working directory
WORKDIR /home/user

# Install basic utilities and updates
RUN apt-get update && \
    apt-get install -y \
    curl \
    wget \
    vim \
    git \
    sudo \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Create a non-root user (optional, for security reasons)
RUN useradd -m -s /bin/bash user && \
    echo "user:user" | chpasswd && \
    adduser user sudo

# Set the default user to 'user' (optional, for non-root execution)
USER user

# Set a default command
CMD ["bash"]
