##############################################################################
Ubuntu
##############################################################################


==============================================================================
Ubuntu 14.04
==============================================================================

1. Prequisitions

::

    # dependencies
    $ sudo apt-get update
    $ sudo apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        git \
        libgoogle-glog-dev \
        libprotobuf-dev \
        protobuf-compiler \
        python-dev \
        python-pip                          
    $ sudo pip install numpy protobuf

    # gpu support
    $ sudo apt-get update && sudo apt-get install wget -y --no-install-recommends
    $ wget "http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_8.0.61-1_amd64.deb"
    $ sudo dpkg -i cuda-repo-ubuntu1404_8.0.61-1_amd64.deb
    $ sudo apt-get update
    $ sudo apt-get install cuda

    # cudnn
    $ CUDNN_URL="http://developer.download.nvidia.com/compute/redist/cudnn/v5.1/cudnn-8.0-linux-x64-v5.1.tgz"
    $ wget ${CUDNN_URL}
    $ sudo tar -xzf cudnn-8.0-linux-x64-v5.1.tgz -C /usr/local
    $ rm cudnn-8.0-linux-x64-v5.1.tgz && sudo ldconfig

    # optional dependencies
    $ sudo apt-get install -y --no-install-recommends libgflags2
    $ sudo apt-get install -y --no-install-recommends \
        libgtest-dev \
        libiomp-dev \
        libleveldb-dev \
        liblmdb-dev \
        libopencv-dev \
        libopenmpi-dev \
        libsnappy-dev \
        openmpi-bin \
        openmpi-doc \
        python-pydot
    $ sudo pip install \
        flask \
        graphviz \
        hypothesis \
        jupyter \
        matplotlib \
        pydot python-nvd3 \
        pyyaml \
        requests \
        scikit-image \
        scipy \
        setuptools \
        tornado


2. caffe2

::

    $ git clone --recursive https://github.com/caffe2/caffe2.git && cd caffe2
    $ make && cd build && sudo make install
    $ python -c 'from caffe2.python import core' 2>/dev/null && echo "Success" || echo "Failure"

