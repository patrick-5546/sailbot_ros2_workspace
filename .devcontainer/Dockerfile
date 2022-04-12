FROM ghcr.io/patrick-5546/sailbot_ros2_workspace/dev:raye

# # ** [Optional] Uncomment this section to install additional packages. **
# ENV DEBIAN_FRONTEND=noninteractive
# RUN apt-get update \
#   && apt-get -y install --no-install-recommends \
#   # Clean up
#   && apt-get autoremove -y \
#   && apt-get clean -y \
#   && rm -rf /var/lib/apt/lists/*
# ENV DEBIAN_FRONTEND=dialog

ENV USER ros
USER ros 
ENV HOME /home/${USER} 

# ROS1 workspace setup
RUN mkdir -p ${HOME}/catkin_ws/src
WORKDIR ${HOME}/catkin_ws
RUN /bin/bash -c "source /opt/ros/${ROS1_DISTRO}/setup.bash; catkin_make"
RUN git clone https://github.com/UBCSailbot/local-pathfinding.git src/local-pathfinding
RUN git clone https://github.com/UBCSailbot/sailbot-msg.git src/sailbot-msg
RUN pip install shapely
RUN /bin/bash -c "source /opt/ros/${ROS1_DISTRO}/setup.bash; catkin_make"

# ROS2 workspace setup
WORKDIR /workspaces/sailbot_ros2_workspace
RUN /bin/bash -c "source /opt/ros/${ROS_DISTRO}/setup.bash; colcon build --merge-install --symlink-install"

# Persist ROS logs
RUN mkdir -p ${HOME}/.ros/log \
    && chown -R ${USER} ${HOME}/.ros/log

# Persist bash history
RUN SNIPPET="export PROMPT_COMMAND='history -a' && export HISTFILE=${HOME}/commandhistory/.bash_history" \
  && mkdir ${HOME}/commandhistory \
  && touch ${HOME}/commandhistory/.bash_history \
  && chown -R ${USER} ${HOME}/commandhistory \
  && echo $SNIPPET >> "/home/${USER}/.bashrc"

# Copy bashrc
COPY config/update_bashrc.sh /sbin/update_bashrc
RUN sudo chmod +x /sbin/update_bashrc ; sudo chown ros /sbin/update_bashrc ; sync ; /bin/bash -c /sbin/update_bashrc ; sudo rm /sbin/update_bashrc

# Place user-specific configuration files in config/user/ and copy them over
#COPY config/user/.vimrc ${HOME}/.vimrc
