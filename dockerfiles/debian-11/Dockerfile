######## INSTALL ########

# Set the base image
FROM        debian:11-slim

# Set environment variables
ENV         USER root
ENV         HOME /pufferpanel
ENV         WINEPREFIX=/pufferpanel/.wine

# Set working directory
WORKDIR     $HOME

# Insert Steam prompt answers
SHELL       ["/bin/bash", "-o", "pipefail", "-c"]
RUN         echo steam steam/question select "I AGREE" | debconf-set-selections \
                && echo steam steam/license note '' | debconf-set-selections

# Update the repository and install SteamCMD
ARG         DEBIAN_FRONTEND=noninteractive
COPY        sources.list /etc/apt/sources.list
RUN         dpkg --add-architecture i386 \
            && apt-get update -y \
            && apt-get install -y --no-install-recommends ca-certificates locales steamcmd wget curl git \
            && rm -rf /var/lib/apt/lists/*

# Add wine support
RUN         mkdir -pm755 /etc/apt/keyrings
RUN         wget -O /etc/apt/keyrings/winehq-archive.key https://dl.winehq.org/wine-builds/winehq.key
RUN         wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/debian/dists/bullseye/winehq-bullseye.sources
RUN         apt-get update
RUN         apt-get  install -y --install-recommends winehq-stable cabextract wine-binfmt
RUN         apt install -y \
                alsa-tools \
                ca-certificates \
                curl \
                dbus \
                dirmngr \
                g++ \
                gcc \
                gdb \
                git \
                git-lfs \
                gnupg \
                gnupg2 \
                gnutls-bin \
                iproute2 \
                jq \
                locales \
                mesa-utils \
                net-tools \
                netcat-openbsd \
                numactl \
                psmisc \
                python3 \
                python3-dev \
                python3-pip \
                sed \
                software-properties-common \
                speex:i386 \
                tar \
                telnet \
                tini \
                tzdata \
                wget \
                winbind \
                xauth \
                xvfb \
                lib32gcc-s1-amd64-cross \
                libgcc-s1-amd64-cross \
                lib32stdc++6 \
                lib32tinfo6 \
                lib32z1 \
                libao-common \
                libasound2 \
                libatk1.0-0 \
                libc++1 \
                libc6 \
                libcurl4 \
                libcurl4-gnutls-dev:i386 \
                libcurl4:i386 \
                libfontconfig1 \
                libgcc1 \
                libgconf-2-4 \
                libgdiplus \
                libgdiplus \
                libgtk2.0-0:i386 \
                libncurses5-dev:i386 \
                libncurses5:i386 \
                libncurses6 \
                libnss-wrapper \
                libnss-wrapper:i386 \
                libnss3 \
                libntlm0 \
                libpangocairo-1.0-0 \
                libpulse-dev \
                libsdl1.2debian \
                libsdl1.2debian:i386 \
                libsdl2-2.0-0 \
                libsdl2-2.0-0:i386 \
                libssl1.1 \
                libssl1.1:i386 \
                libstdc++6 \
                libstdc++6:i386 \
                libtcmalloc-minimal4 \
                libtinfo6:i386 \
                libxcomposite1 \
                libxcursor1 \
                libxdamage1 \
                libxi6 \
                libxrandr2 \
                libxss1 \
                libxtst6

# Set up Winetricks
RUN	            wget -q -O /usr/sbin/winetricks https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
RUN             chmod +x /usr/sbin/winetricks

# Set Display
ENV             WINEDLLOVERRIDES="mscoree,mshtml="
ENV             DISPLAY=:0
ENV             DISPLAY_WIDTH=1024
ENV             DISPLAY_HEIGHT=768
ENV             DISPLAY_DEPTH=16

# Add unicode support
RUN         locale-gen en_US.UTF-8
ENV         LANG 'en_US.UTF-8'
ENV         LANGUAGE 'en_US:en'

# Create symlink for executable
RUN         ln -s /usr/games/steamcmd /usr/bin/steamcmd

# Update SteamCMD and verify latest version
RUN         steamcmd +quit

# Set default command
ENTRYPOINT  ["steamcmd"]
CMD         ["+help", "+quit"]
