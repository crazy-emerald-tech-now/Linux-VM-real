# Use the latest Ubuntu image as the base
FROM ubuntu:latest

# Set a working directory
WORKDIR /workspace

# Update the package lists and install some basic tools
RUN apt-get update && apt-get install -y \
    curl \
    git \
    nano \
    && rm -rf /var/lib/apt/lists/*

# Set the default command to launch a bash shell
CMD ["/bin/bash"]
