FROM nvidia/cuda:11.8.0-devel-ubuntu20.04
MAINTAINER FrancescoC

## for apt to be noninteractive
ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

# ------------------------------------------------------------------------------------
# Basic dependencies
# ------------------------------------------------------------------------------------
RUN echo "tzdata tzdata/Areas select Europe" > /tmp/preseed.txt; \
    echo "tzdata tzdata/Zones/Europe select Rome" >> /tmp/preseed.txt; \
    debconf-set-selections /tmp/preseed.txt && \
    apt-get update && \
    apt-get install --no-install-recommends -y tzdata git wget cmake make build-essential g++ \
     python3 python3-pip python3-dev python3-numpy python3-venv && \
    rm -rf /var/lib/apt/lists/*

# ------------------------------------------------------------------------------------
# CUDNN
# ------------------------------------------------------------------------------------

COPY Docker/Scripts/install_cudnn.sh /tmp/install_cudnn.sh
RUN /bin/bash -c "chmod +x /tmp/install_cudnn.sh && \
    bash /tmp/install_cudnn.sh"



# ------------------------------------------------------------------------------------
# OPENCV
# ------------------------------------------------------------------------------------

COPY Docker/Scripts/install_Opencv.sh /tmp/install_Opencv.sh
RUN /bin/bash -c "chmod +x /tmp/install_Opencv.sh && \
    bash /tmp/install_Opencv.sh"


# ------------------------------------------------------------------------------------
# Pytorch
# ------------------------------------------------------------------------------------

RUN /bin/bash -c "pip install --upgrade pip && \
    pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118"


## Create directories
RUN /bin/bash -c "mkdir -p ~/Archive/Shared"

# Security updates
# https://security.snyk.io/vuln/SNYK-UBUNTU1804-OPENSSL-3314796
RUN apt upgrade --no-install-recommends -y openssl tar


# ------------------------------------------------------------------------------------
# Thirdparty-RCF
# ------------------------------------------------------------------------------------
WORKDIR /root/Programs

# Copy the required source packages into the workspace ROS
COPY Thirdparty/RCF_Pytorch_Updated/ ./RCF_Pytorch_Updated
RUN /bin/bash -c "cd RCF_Pytorch_Updated && pip install -r requirements.txt"

COPY Thirdparty/segment-anything/ ./segment-anything
RUN /bin/bash -c "pip install pycocotools matplotlib onnxruntime onnx"

COPY Thirdparty/Zero-DCE-anything/ ./Zero-DCE
