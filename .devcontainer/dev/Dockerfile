FROM ghcr.io/patrick-5546/sailbot_ros2_workspace/base:raye
LABEL org.opencontainers.image.source=https://github.com/patrick-5546/sailbot_ros2_workspace

# https://github.com/athackst/dockerfiles/blob/main/ros2/eloquent.Dockerfile
# curl and wget?
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
  && apt-get -y install --no-install-recommends \
  bash-completion \
  cmake \
  curl \
  gdb \
  pylint3 \
  python-autopep8 \
  python3-argcomplete \
  python3-pip \
  vim \
  wget \
  # Clean up
  && apt-get autoremove -y \
  && apt-get clean -y \
  && rm -rf /var/lib/apt/lists/*
ENV DEBIAN_FRONTEND=dialog

ARG USERNAME=ros
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create a non-root user
RUN groupadd --gid $USER_GID $USERNAME \
  && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
  # [Optional] Add sudo support for the non-root user
  && apt-get update \
  && apt-get install -y sudo \
  && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME\
  && chmod 0440 /etc/sudoers.d/$USERNAME \
  # Cleanup
  && rm -rf /var/lib/apt/lists/* \
  && echo "source /usr/share/bash-completion/completions/git" >> /home/$USERNAME/.bashrc \
ENV DEBIAN_FRONTEND=

WORKDIR /home/${USERNAME}
