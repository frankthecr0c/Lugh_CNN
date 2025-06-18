
# Docker

## HOST Prerequisites

    sudo apt install  nvidia-docker2 nvidia-container-toolkit
For further details, please visit [https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html
](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html
)
## Image Building

change directory on top of this repository ```/yourpaths/ArborAI``` and build the image using
the Dockerfile
```
docker build -t arborai:1.0 -f Docker/Dockerfile --build-arg CCAP=<CCAP_version>  .
```

where 
```<CCAP_version>``` is GPU dependant (it must be compatible with CUDA11, and it can be seen with 

```
nvidia-smi --query-gpu=compute_cap --format=csv,noheader
``` 

or by looking for your GPU model 
[here](https://developer.nvidia.com/cuda-gpus)).
N.B. in our case ```CCAP=7.5```, therefore the provided Docker image can be used just with NVIDIA 2060.


# Container Running
Then run the container with the following commands:

    ddocker run -td -i --privileged --net=host --name=ArborAi arborai:1.0

Running with X11 (recommended)

    docker run -td -i --privileged --net=host --name=ArborAi  -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -e "QT_X11_NO_MITSHM=1" -h $HOSTNAME -v $XAUTHORITY:/root/.Xauthority:rw  --runtime=nvidia --gpus all --env="NVIDIA_DRIVER_CAPABILITIES=all" arborai:1.0

## After Installation checks

Start the container and open a bash shell: 
```
docker exec -it ArborAi bash
```
and perform some tests:
- python3 checks: torch with cuda , opencv <br>
    ```console
    Python 3.8.10 (default, Jan 17 2025, 14:40:23) 
    [GCC 9.4.0] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import torch
    >>> import cv2
    >>> torch.cuda.is_available()
    True
    >>> print(cv2.cuda.getCudaEnabledDeviceCount())
    1
    ```
    the latest results (1) depends on your hardware, in this example we had one graphic card cuda enabled.
