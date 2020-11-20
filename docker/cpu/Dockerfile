FROM ubuntu:16.04

RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        git \
        wget \
        libatlas-base-dev \
        libboost-all-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libopencv-dev \
        libprotobuf-dev \
        libsnappy-dev \
        libyaml-cpp-dev \
        libopenblas-dev \
        protobuf-compiler \
        python-dev \
        python-numpy \
        python-pip \
        python-setuptools \
        python-scipy && \
    rm -rf /var/lib/apt/lists/*

ENV CAFFE_ROOT=/opt
WORKDIR $CAFFE_ROOT

ENV CLONE_TAG=v1.2.1

RUN git clone -b ${CLONE_TAG} --depth 1 https://github.com/Xilinx/Vitis-AI.git . && \
    pip install --upgrade pip && \
    cd AI-Model-Zoo/caffe-xilinx && \
    cd python && for req in $(cat requirements.txt) pydot; do pip install $req; done && cd .. && \
    touch Makefile.config && \ 
    echo "CPU_ONLY := 1" >> Makefile.config && \ 
    echo "BLAS := open" >> Makefile.config && \ 
    echo "WITH_PYTHON_LAYER := 1" >> Makefile.config && \ 
    echo "INCLUDE_DIRS := /usr/include/python2.7 /usr/lib/python2.7/dist-packages/numpy/core/include /usr/local/include" >> Makefile.config && \  
    echo "LIBRARY_DIRS := /usr/lib /usr/local/lib /usr/lib" >> Makefile.config && \  
    echo "BUILD_DIR := build" >> Makefile.config && \  
    echo "DISTRIBUTE_DIR := distribute" >> Makefile.config && \ 
    echo "Q ?= @" >> Makefile.config && \  
    echo "LIBRARY_NAME_SUFFIX := -deephi" >> Makefile.config && \ 
    echo "DPU_ACCURACY := 1" >> Makefile.config && \ 
    make -j"$(nproc)"

ENV PYCAFFE_ROOT $CAFFE_ROOT/python
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig

WORKDIR /workspace


# mkdir build && cd build && \
# cmake -DCPU_ONLY=1 .. && \