FROM ubuntu:focal

# basic args
ENV ROS_DISTRO=noetic
ARG USERNAME=user
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Add the following labels
LABEL org.opencontainers.image.description="Docker Development Container"
LABEL org.opencontainers.image.title="ROS1Dev"
LABEL org.opencontainers.image.vendor="Tarun R"
LABEL org.opencontainers.image.source="https://github.com/rtarun1/DockerForROS2Development"
LABEL maintainer="tarun.ramak@gmail.com"
LABEL org.opencontainers.image.licenses="MIT"
# handle default shell
SHELL ["/bin/bash", "-c"]
ENV SHELL=/bin/bash

# setup timezone
RUN echo 'Asia/Kolkata' > /etc/timezone && \
    ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime && \
    apt-get update && \
    apt-get install -q -y --no-install-recommends tzdata && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -q -y --no-install-recommends \
    dirmngr \
    gnupg2 \
    && rm -rf /var/lib/apt/lists/*

# setup keys
RUN set -eux; \
    key='C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654'; \
    export GNUPGHOME="$(mktemp -d)"; \
    gpg --batch --keyserver keyserver.ubuntu.com --recv-keys "$key"; \
    mkdir -p /usr/share/keyrings; \
    gpg --batch --export "$key" > /usr/share/keyrings/ros1-latest-archive-keyring.gpg; \
    gpgconf --kill all; \
    rm -rf "$GNUPGHOME"

# setup sources.list
RUN echo "deb [ signed-by=/usr/share/keyrings/ros1-latest-archive-keyring.gpg ] http://packages.ros.org/ros/ubuntu focal main" > /etc/apt/sources.list.d/ros1-latest.list

# setup environment
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8

# Create non root user with sudo privilege
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME
USER $USERNAME

ENV ROS_DISTRO=$ROS_DISTRO

# install ros packages
RUN sudo apt-get update && sudo apt-get install -y --no-install-recommends \
    ros-$ROS_DISTRO-desktop-full=1.5.0-1* \
    && sudo rm -rf /var/lib/apt/lists/*

# install bootstrap tools
RUN sudo apt-get update && sudo apt-get install --no-install-recommends -y \
    build-essential \
    python3-rosdep \
    python3-rosinstall \
    python3-vcstools \
    git \
    && sudo rm -rf /var/lib/apt/lists/*

# bootstrap rosdep
RUN sudo rosdep init && \
    rosdep update --rosdistro $ROS_DISTRO

# setup entrypoint
COPY ./ros_entrypoint.sh /

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]