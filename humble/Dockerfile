FROM ubuntu:jammy

# basic args
ARG ROS_DISTRO=humble
ARG USERNAME=container_user
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Add the following labels
LABEL org.opencontainers.image.description="ROS2 Development Container"
LABEL org.opencontainers.image.title="ROS2DevCon"
LABEL org.opencontainers.image.vendor="Soham Patil"
LABEL org.opencontainers.image.source="https://github.com/soham2560/DockerForROSDevelopment"
LABEL maintainer="sohampatil45939@gmail.com"
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

# install packages
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
    gpg --batch --export "$key" > /usr/share/keyrings/ros2-latest-archive-keyring.gpg; \
    gpgconf --kill all; \
    rm -rf "$GNUPGHOME"

# setup sources.list
RUN echo "deb [ signed-by=/usr/share/keyrings/ros2-latest-archive-keyring.gpg ] http://packages.ros.org/ros2/ubuntu jammy main" > /etc/apt/sources.list.d/ros2-latest.list

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

# install ros2 packages
RUN sudo apt-get update && sudo apt-get install -y --no-install-recommends \
    ros-$ROS_DISTRO-desktop=0.10.0-1* \
    && sudo rm -rf /var/lib/apt/lists/*

# setup entrypoint
COPY ./ros_entrypoint.sh /

ENTRYPOINT ["/ros_entrypoint.sh"]

# install bootstrap tools
RUN sudo apt-get update && sudo apt-get install --no-install-recommends -y \
    build-essential \
    git \
    bash-completion \
    python3-colcon-common-extensions \
    python3-colcon-mixin \
    python3-rosdep \
    python3-vcstool \
    python3-pip \
    && sudo rm -rf /var/lib/apt/lists/*

# bootstrap rosdep
RUN sudo rosdep init && \
    rosdep update --rosdistro $ROS_DISTRO

# setup colcon mixin and metadata
RUN sudo colcon mixin add default \
    https://raw.githubusercontent.com/colcon/colcon-mixin-repository/master/index.yaml && \
    sudo colcon mixin update && \
    sudo colcon metadata add default \
    https://raw.githubusercontent.com/colcon/colcon-metadata-repository/master/index.yaml && \
    sudo colcon metadata update

RUN sudo pip3 install -U \
    argcomplete

# echo sources
RUN echo 'source /usr/share/bash-completion/bash_completion' | sudo tee -a ~/.bashrc > /dev/null && \
    echo 'source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash' | sudo tee -a ~/.bashrc > /dev/null && \
    echo "source /opt/ros/${ROS_DISTRO}/setup.bash" | sudo tee -a ~/.bashrc > /dev/null && \
    echo 'eval "$(register-python-argcomplete3 ros2)"' | sudo tee -a ~/.bashrc > /dev/null && \
    echo 'eval "$(register-python-argcomplete3 colcon)"' | sudo tee -a ~/.bashrc > /dev/null