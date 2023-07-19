ARG PYTORCH="1.6.0"
ARG CUDA="10.1"
ARG CUDNN="7"

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

ENV TORCH_CUDA_ARCH_LIST="6.0 6.1 7.0+PTX"
ENV TORCH_NVCC_FLAGS="-Xfatbin -compress-all"
ENV CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"

# https://jdhao.github.io/2022/05/05/nvidia-apt-repo-public-key-error-fix/
RUN rm /etc/apt/sources.list.d/cuda.list
RUN rm /etc/apt/sources.list.d/nvidia-ml.list
RUN apt-key del 7fa2af80
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

RUN apt-get update && apt-get install -y git ninja-build libglib2.0-0 libsm6 libxrender-dev libxext6 \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN conda clean --all

RUN apt update && apt install ffmpeg libsm6 libxext6 -y

# Install mmsegmentation
RUN pip install mmcv-full==latest+torch1.6.0+cu101 -f https://download.openmmlab.com/mmcv/dist/index.html
RUN git clone https://github.com/open-mmlab/mmsegmentation.git /mmsegmentation
WORKDIR /mmsegmentation
RUN git checkout v0.13.0
RUN pip install -r requirements.txt
RUN pip install --no-cache-dir -e .
