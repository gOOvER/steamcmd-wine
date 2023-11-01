######## INSTALL ########

# Set the base image
FROM        ubuntu:22.04

# Set environment variables
ENV         USER root
ENV         HOME /root

# Set working directory
WORKDIR     $HOME

# Insert Steam prompt answers
SHELL       ["/bin/bash", "-o", "pipefail", "-c"]
RUN         echo steam steam/question select "I AGREE" | debconf-set-selections \
            && echo steam steam/license note '' | debconf-set-selections

# Update the repository and install SteamCMD
ARG         DEBIAN_FRONTEND=noninteractive
RUN         dpkg --add-architecture i386 \
            && apt-get update -y \
            && apt-get install -y --no-install-recommends ca-certificates locales steamcmd wget curl git \
            && rm -rf /var/lib/apt/lists/*

# Add wine support
RUN         mkdir -pm755 /etc/apt/keyrings
RUN         wget -O /etc/apt/keyrings/winehq-archive.key https://dl.winehq.org/wine-builds/winehq.key
RUN         wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/ubuntu/dists/jammy/winehq-jammy.sources
RUN         apt-get update
RUN         apt-get  install -y --install-recommends winehq-stable

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
