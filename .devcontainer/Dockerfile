ARG ROS_DISTRO=rolling

FROM ros:${ROS_DISTRO}-ros-base

ARG USERNAME=USERNAME
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG ROS_DISTRO

# Delete user if it exists in container (e.g Ubuntu Noble: ubuntu)
RUN if id -u $USER_UID ; then userdel `id -un $USER_UID` ; fi

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y curl sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

RUN install -dm 755 /etc/apt/keyrings && \
    curl -fSs https://mise.jdx.dev/gpg-key.pub | sudo tee /etc/apt/keyrings/mise-archive-keyring.pub 1> /dev/null && \
    echo "deb [signed-by=/etc/apt/keyrings/mise-archive-keyring.pub arch=amd64] https://mise.jdx.dev/deb stable main" | sudo tee /etc/apt/sources.list.d/mise.list

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y  \
        python3-pip python-is-python3 \
        mise \
        ros-${ROS_DISTRO}-launch ros-${ROS_DISTRO}-ur && \
    apt-get autoremove -y

ENV SHELL="/bin/bash"
USER $USERNAME

RUN mise use --global \
    node@24 \
    uv

RUN echo 'eval "$(mise activate bash)"' >> ~/.bashrc && \
    echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> ~/.bashrc

CMD ["/bin/bash"]
