####################################
# Ubuntu ROS2 Humble Gazebo Garden
####################################

FROM ghcr.io/soham2560/humble:latest AS base

# Environment setup
ENV GZ_VERSION=fortress

# install necessary tools
RUN sudo apt-get update && sudo apt-get install --no-install-recommends -y \
    lsb-release \
    wget \
    gnupg \
    && sudo rm -rf /var/lib/apt/lists/*

# Install gazebo fortress from osrf
RUN sudo curl https://packages.osrfoundation.org/gazebo.gpg --output /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null \
    && sudo apt-get -qq update \
    && sudo xargs apt-get install --no-install-recommends -y \
    ignition-fortress \
    && sudo rm -rf /var/lib/apt/lists/*

RUN sudo apt-get update && sudo apt-get install -y ros-$ROS_DISTRO-ros-gz