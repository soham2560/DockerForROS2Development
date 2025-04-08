####################################
# Ubuntu ROS2 Iron Gazebo Garden
####################################

FROM ghcr.io/soham2560/iron:latest AS base

# Environment setup
ENV GZ_VERSION=garden

# install necessary tools
RUN sudo apt-get update && sudo apt-get install --no-install-recommends -y \
    lsb-release \
    wget \
    gnupg \
    && sudo rm -rf /var/lib/apt/lists/*
# Install gazebo garden from osrf
RUN sudo bash -c 'wget https://packages.osrfoundation.org/gazebo.gpg -O /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg' \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee -a /etc/apt/sources.list.d/gazebo-stable.list > /dev/null \
    && sudo apt-get -qq update \
    && sudo xargs apt-get install --no-install-recommends -y \
    gz-garden \
    && sudo rm -rf /var/lib/apt/lists/*

RUN sudo apt-get update && sudo apt-get install -y ros-$ROS_DISTRO-ros-gzgarden-*