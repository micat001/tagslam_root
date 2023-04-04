FROM  amarburg/trisect-desktop:noetic-headless-perception

# Install dependencies required to build g2o
RUN apt update && \
    apt install -y \
    libsuitesparse-dev && \
    apt clean

# ROS BASE
# install bootstrap tools
RUN apt update && apt install -y \
    python3-vcstools \
    python3-catkin-tools \
    python3-osrf-pycommon

# Add the borglab PPA and install GTSAM
RUN apt-add-repository ppa:borglab/gtsam-release-4.1 && \
    apt update && \
    apt install -y \
    libgtsam-dev \
    libgtsam-unstable-dev \
    libeigen3-dev \
    libtbb-dev

# Clone the repo:
WORKDIR /root/build
RUN git clone https://github.com/micat001/tagslam_root.git && \
    cd tagslam_root && \
    vcs import --recursive < tagslam_root.repos

# Build the workspace
RUN /bin/bash -c "source /opt/ros/noetic/setup.bash && \
    cd /root/build/tagslam_root && \
    catkin config -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCATKIN_ENABLE_TESTING=False && \
    catkin build"

RUN echo "source /root/build/tagslam_root/devel/setup.bash" >> ~/.bashrc
