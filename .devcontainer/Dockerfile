# OS
ARG VARIANT=bullseye
FROM --platform=linux/amd64 mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}


# Instalar dependencias necesarias para añadir un nuevo repositorio
RUN apt-get update \
    && apt-get install -y wget gnupg2 software-properties-common lsb-release

# Agregar el repositorio de Microsoft para VS Code
RUN wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | gpg --dearmor > microsoft.asc.gpg \
    && mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/ \
    && wget -q https://packages.microsoft.com/config/debian/$(lsb_release -rs)/prod.list \
    && mv prod.list /etc/apt/sources.list.d/microsoft-prod.list

# Actualizar la lista de paquetes después de añadir el repositorio de Microsoft
RUN apt-get update

# Instalar Visual Studio Code y Python
RUN apt-get install -y code python3 python3-pip



# Desktop lite
COPY library-scripts/desktop-lite-debian.sh /tmp/library-scripts/
COPY fluxbox /home/vscode/.fluxbox/
RUN apt-get update \
    && bash /tmp/library-scripts/desktop-lite-debian.sh \
    && chown -R vscode /home/vscode
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