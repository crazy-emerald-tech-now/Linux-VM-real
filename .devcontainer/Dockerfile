# Start with a base Ubuntu image
FROM ubuntu:22.04

# Avoid interactive prompts during installation
ARG DEBIAN_FRONTEND=noninteractive

# Install dependencies for a lightweight desktop, VNC, and Chrome
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    # Desktop and VNC
    xfce4 \
    xfce4-goodies \
    novnc \
    # Dependencies for Chrome
    libxss1 \
    libappindicator1 \
    libindicator7 \
    && rm -rf /var/lib/apt/lists/*

# Install Google Chrome
RUN curl -sS -o - https://dl-ssl.google.com/linux/linux_signing_key.pub | gpg --dearmor | tee /usr/share/keyrings/google-chrome.gpg >/dev/null \
    && echo "deb [arch=amd64 signed-by=/usr/share/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list \
    && apt-get update \
    && apt-get install -y google-chrome-stable

# Create a non-root user
RUN useradd --create-home --shell /bin/bash vscode

# Copy the startup script
COPY vnc-startup.sh /usr/local/bin/vnc-startup.sh
RUN chmod +x /usr/local/bin/vnc-startup.sh

USER vscode
WORKDIR /home/vscode

# Expose the VNC port
EXPOSE 6080

CMD ["/usr/local/bin/vnc-startup.sh"]```

**Explanation of the `Dockerfile`:**

*   This `Dockerfile` installs the XFCE desktop environment, a lightweight and fast option.
*   It also installs `noVNC`, a VNC client that runs in the browser.
*   The necessary steps to add the Google Chrome repository and install it are included.
*   A non-root user `vscode` is created for better security.
*   A startup script `vnc-startup.sh` is copied into the image and made executable.

### **Step 4: Create the startup script**

In the `.devcontainer` directory, create a file named `vnc-startup.sh`. This script will start the VNC server and the desktop environment.

```bash
#!/bin/bash
set -e

# Start the VNC server
/usr/bin/websockify --web=/usr/share/novnc/ 6080 localhost:5901 &

# Start XFCE desktop environment
x11vnc -display :1 -forever -nopw -create &

# Launch a terminal
xfce4-terminal
