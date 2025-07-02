FROM nvidia/cuda:12.1.0-devel-ubuntu20.04 AS dev

ARG ROS_VERSION=noetic
ARG DEBIAN_FRONTEND=noninteractive
SHELL ["/bin/bash", "-c"]

################# ROS Install #################
RUN apt-get update && apt-get install -y --no-install-recommends \
      curl gnupg lsb-release git build-essential nano unzip wget \
 && curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
      | gpg --dearmor -o /usr/share/keyrings/ros-archive-keyring.gpg \
 && echo "deb [arch=amd64 signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] \
      http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" \
      > /etc/apt/sources.list.d/ros-latest.list \
 && apt-get update && apt-get install -y --no-install-recommends \
      ros-${ROS_VERSION}-desktop-full \
      python3-pip python3-rosdep python3-catkin-tools \
 && rosdep init && rosdep update \
 && echo "source /opt/ros/${ROS_VERSION}/setup.bash" >> /root/.bashrc \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

################# Depen #################
RUN apt-get update && apt-get install -y --no-install-recommends \
      libpcl-dev libeigen3-dev \
      ros-noetic-grid-map ros-noetic-grid-map-core \
      ros-noetic-pcl-ros \
 && apt-get clean && rm -rf /var/lib/apt/lists/*


################# workspace #################
RUN mkdir -p /root/rsl_travel_ws/src

RUN cd /root/rsl_travel_ws/src && \
    git clone -b noetic https://github.com/leggedrobotics/kindr.git && \
    git clone https://github.com/anybotics/kindr_ros.git && \
    git clone https://github.com/leggedrobotics/any_node.git && \
    git clone https://github.com/ANYbotics/message_logger.git

WORKDIR /root/rsl_travel_ws

RUN /bin/bash -c "source /opt/ros/${ROS_VERSION}/setup.bash && \
                  catkin config --extend /opt/ros/${ROS_VERSION}"

ENV USER=root \
    HOME=/root \
    CMAKE_PREFIX_PATH=/opt/libtorch:$CMAKE_PREFIX_PATH \
    LD_LIBRARY_PATH=/opt/libtorch/lib:$LD_LIBRARY_PATH

RUN echo 'source /root/rsl_travel_ws/devel/setup.bash' >> /root/.bashrc

WORKDIR /root/rsl_travel_ws
