FROM nvidia/cuda:12.2.0-base-ubuntu22.04
FROM osrf/ros:humble-desktop-full

ENV NVIDIA_VISIBLE_DEVICES \
    ${NVIDIA_VISIBLE_DEVICES:-all}
ENV NVIDIA_DRIVER_CAPABILITIES \
    ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics
ENV QT_X11_NO_MITSHM=1
ENV EDITOR=nano
ENV XDG_RUNTIME_DIR=/tmp

RUN apt-get update && apt-get install -y \
    cmake \
    gazebo \
    libglu1-mesa-dev \
    vim \
    python3-pip \
    python3-pydantic \
    python3-venv \
    ros-humble-gazebo-ros \
    ros-humble-gazebo-ros-pkgs \
    ros-humble-joint-state-publisher \
    ros-humble-robot-localization \
    ros-humble-plotjuggler-ros \
    ros-humble-robot-state-publisher \
    ros-humble-ros2bag \
    ros-humble-rosbag2-storage-default-plugins \
    ros-humble-rqt-tf-tree \
    ros-humble-rmw-fastrtps-cpp \
    ros-humble-rmw-cyclonedds-cpp \
    ros-humble-slam-toolbox \
    ros-humble-turtlebot3 \
    ros-humble-turtlebot3-msgs \
    ros-humble-twist-mux \
    ros-humble-usb-cam \
    ros-humble-xacro \
    ruby-dev \
    rviz \
    tmux \
    wget \
    xorg-dev \
    x11-apps \
    build-essential \ 
    libboost-program-options-dev \ 
    libusb-1.0-0-dev \ 
    ros-humble-tf-transformations \ 
    libjsoncpp-dev

RUN apt-get install -y  curl lsb-release gnupg
RUN curl https://packages.osrfoundation.org/gazebo.gpg --output /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg 
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
RUN echo " CURL DONE"

RUN  apt-get update 
RUN  apt-get install -y gz-harmonic
RUN echo "Ignition Installed"

RUN pip3 install setuptools==58.2.0

RUN wget https://github.com/openrr/urdf-viz/releases/download/v0.38.2/urdf-viz-x86_64-unknown-linux-gnu.tar.gz && \
    tar -xvzf urdf-viz-x86_64-unknown-linux-gnu.tar.gz -C /usr/local/bin/ && \
    chmod +x /usr/local/bin/urdf-viz && \
    rm -f urdf-viz-x86_64-unknown-linux-gnu.tar.gz

# The installer requires curl (and certificates) to download the release archive
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates

# Download the latest installer
ADD https://astral.sh/uv/install.sh /uv-installer.sh

# Run the installer then remove it
RUN sh /uv-installer.sh && rm /uv-installer.sh

# Ensure the installed binary is on the `PATH`
ENV PATH="/root/.local/bin/:$PATH"

RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

RUN echo "export DISABLE_AUTO_TITLE=true" >> /root/.bashrc
RUN echo 'LC_NUMERIC="en_US.UTF-8"' >> /root/.bashrc
RUN echo "source /opt/ros/humble/setup.bash" >> /root/.bashrc
RUN echo "source /usr/share/gazebo/setup.sh" >> /root/.bashrc

RUN echo 'alias rosdi="rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y"' >> /root/.bashrc
RUN echo 'alias cbuild="colcon build --symlink-install"' >> /root/.bashrc
RUN echo 'alias ssetup="source ./install/local_setup.bash"' >> /root/.bashrc
RUN echo 'alias cyclone="export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp"' >> /root/.bashrc
RUN echo 'alias fastdds="export RMW_IMPLEMENTATION=rmw_fastrtps_cpp"' >> /root/.bashrc
RUN echo 'export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp' >> /root/.bashrc

RUN echo "autoload -U bashcompinit" >> /root/.bashrc
RUN echo "bashcompinit" >> /root/.bashrc
RUN echo 'eval "$(register-python-argcomplete3 ros2)"' >> /root/.bashrc
RUN echo 'eval "$(register-python-argcomplete3 colcon)"' >> /root/.bashrc
