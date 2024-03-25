# OS
ARG VARIANT=bullseye
FROM --platform=linux/amd64 mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

# Instala las herramientas necesarias
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    gpg \
    software-properties-common \
    apt-transport-https \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Descarga e instala Visual Studio Code
RUN wget https://vscode.download.prss.microsoft.com/dbazure/download/stable/863d2581ecda6849923a2118d93a088b0745d9d6/code_1.87.2-1709912201_amd64.deb -O /tmp/code.deb && \
    dpkg -i /tmp/code.deb || apt-get install -fy \
    && rm -rf /var/lib/apt/lists/*

# Instalar dependencias necesarias para añadir un nuevo repositorio
RUN apt-get update && apt-get install -y wget gnupg2 software-properties-common lsb-release \
    && rm -rf /var/lib/apt/lists/*

# Agregar el repositorio de Microsoft para VS Code
RUN wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | gpg --dearmor > microsoft.asc.gpg \
    && mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/ \
    && wget -q https://packages.microsoft.com/config/debian/$(lsb_release -rs)/prod.list \
    && mv prod.list /etc/apt/sources.list.d/microsoft-prod.list \
    && apt-get update \
    && rm -rf /var/lib/apt/lists/*

# Desktop lite
COPY library-scripts/desktop-lite-debian.sh /tmp/library-scripts/
COPY fluxbox /home/vscode/.fluxbox/
RUN apt-get update && bash /tmp/library-scripts/desktop-lite-debian.sh \
    && chown -R vscode /home/vscode \
    && rm -rf /var/lib/apt/lists/*
ENV DBUS_SESSION_BUS_ADDRESS="autolaunch:" \
    VNC_RESOLUTION="1440x768x16" \
    VNC_DPI="96" \
    VNC_PORT="5901" \
    NOVNC_PORT="6080" \
    DISPLAY=":1" \
    LANG="en_US.UTF-8" \
    LANGUAGE="en_US.UTF-8"
ENTRYPOINT ["/usr/local/share/desktop-init.sh"]
CMD ["sleep", "infinity"]
