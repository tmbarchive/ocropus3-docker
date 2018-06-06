
Description
===========

Docker container for running OCRopus commands. This bundles up:

    - CUDA and PyTorch
    - A Python 2.7 Conda installation
    - The major modules from OCRopus3

PyTorch is currently at version 0.3.1, since OCRopus hasn't been converted yet
to PyTorch 0.4.

You need to set a Jupyter password (see `jupyter_config/README`) and
a VNC password (see `vnc_config/README`).

To clone this repository, use the `--recursive` flag:

    git clone --recursive git@github.com:tmbdev/ocropus3-docker.git
    cd ocropus3-docker
    ./build

The `ocropus3-docker` repository builds a Docker container that you can use to run OCRopus3 on any platform.

- `./build` -- build the container
- `./ocropy` -- run the container

The container automatically starts a VNC server for graphical output. Inside the container is a complete OCRopus3 installation.

# Docker Container

Some notes on the Docker container:

- the pytorch version is 0.3.1; OCRopus hasn't been ported to 0.4 yet
- the Python installation is in /opt/conda, separate from the regular Ubuntu installation


```python
!head Dockerfile; echo ...; tail Dockerfile
```

    FROM nvidia/cuda:9.0-base
    #FROM nvidia/cuda:9.1-base
    #FROM nvidia/cuda:9.2-devel-ubuntu18.04
    MAINTAINER Tom Breuel <tmbdev@gmail.com>
    
    ENV DEBIAN_FRONTEND noninteractive
    ENV DEBCONF_NONINTERACTIVE_SEEN true
    
    RUN apt-get -y update
    
    ...
    ADD scripts/* /usr/local/bin/
    
    RUN true \
        && echo ". /opt/conda/etc/profile.d/conda.sh" >> $HOME/.bashrc \
        && echo "conda activate base" >> $HOME/.bashrc \
        && chown -R $USER.$USER $HOME
    RUN echo 'user ALL=(ALL:ALL) NOPASSWD:ALL' >> /etc/sudoers
    
    USER $UID
    ENTRYPOINT ["runcmd"]



```python
!./build > log 2>&1 log
! head log; echo ...; tail log
```

    
    
    
    
    
    Step 1/67 : FROM nvidia/cuda:9.0-base
     ---> f3a8582463d4
    Step 2/67 : MAINTAINER Tom Breuel <tmbdev@gmail.com>
     ---> Using cache
     ---> 488754540ac4
    ...
     ---> Using cache
     ---> e4114a076054
    Step 66/67 : USER $UID
     ---> Using cache
     ---> 10c9fd428517
    Step 67/67 : ENTRYPOINT ["runcmd"]
     ---> Using cache
     ---> 1ccdca692521
    Successfully built 1ccdca692521
    Successfully tagged ocropy:latest


Segmentation
============

Train a model with:

    ./ocropy ocroseg-train -d http://storage.googleapis.com/lpr-ocr/uw3-framed-lines.tgz

Models will be saved in the current directory.

You can view the training progress by connecting using VNC:

    xtightvncviewer :99

Models are saved in the current directory by default.


```python
!./ocropy ocroseg-train --maxtrain 10 -d http://storage.googleapis.com/lpr-ocr/uw3-framed-lines.tgz
```

    + ocroseg-train --maxtrain 10 -d http://storage.googleapis.com/lpr-ocr/uw3-framed-lines.tgz
    raw sample:
    __key__ 'A001BIN'
    __source__ 'http://storage.googleapis.com/lpr-ocr/uw3-framed-lines.tgz'
    lines.png float32 (3300, 2592)
    png float32 (3300, 2592)
    
    preprocessed sample:
    __key__ <type 'list'> ['A00BBIN']
    __source__ <type 'list'> ['http://storage.googleapis.com/lpr-ocr/uw3-framed-lines.tgz
    input float32 (1, 3300, 2592, 1)
    mask float32 (1, 3300, 2592, 1)
    output float32 (1, 3300, 2592, 1)
    
    ntrain 0
    model:
    Sequential(
      (0): Conv2d(1, 10, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
      (1): BatchNorm2d(10, eps=1e-05, momentum=0.1, affine=True)
      (2): ReLU()
      (3): MaxPool2d(kernel_size=(2, 2), stride=(2, 2), dilation=(1, 1), ceil_mode=False)
      (4): Conv2d(10, 20, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
      (5): BatchNorm2d(20, eps=1e-05, momentum=0.1, affine=True)
      (6): ReLU()
      (7): MaxPool2d(kernel_size=(2, 2), stride=(2, 2), dilation=(1, 1), ceil_mode=False)
      (8): Conv2d(20, 40, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
      (9): BatchNorm2d(40, eps=1e-05, momentum=0.1, affine=True)
      (10): ReLU()
      (11): LSTM2(
        (hlstm): RowwiseLSTM(
          (lstm): LSTM(40, 20, bidirectional=1)
        )
        (vlstm): RowwiseLSTM(
          (lstm): LSTM(40, 20, bidirectional=1)
        )
      )
      (12): Conv2d(40, 20, kernel_size=(1, 1), stride=(1, 1))
      (13): BatchNorm2d(20, eps=1e-05, momentum=0.1, affine=True)
      (14): ReLU()
      (15): LSTM2(
        (hlstm): RowwiseLSTM(
          (lstm): LSTM(20, 20, bidirectional=1)
        )
        (vlstm): RowwiseLSTM(
          (lstm): LSTM(40, 20, bidirectional=1)
        )
      )
      (16): Conv2d(40, 1, kernel_size=(1, 1), stride=(1, 1))
      (17): Sigmoid()
    )
    
    0 0 ['A009BIN'] 0.2962794 ['A009BIN'] 0.54305035 0.5665481 lr 0.03
    QStandardPaths: XDG_RUNTIME_DIR not set, defaulting to '/tmp/runtime-user'
    Qt: XKEYBOARD extension not present on the X server.
    1 1 ['A006BIN'] 0.29043648 ['A006BIN'] 0.5404598 0.56086785 lr 0.03
    2 2 ['A008BIN'] 0.28884253 ['A008BIN'] 0.53802246 0.56062347 lr 0.03
    3 3 ['A00CBIN'] 0.2859728 ['A00CBIN'] 0.535628 0.55684537 lr 0.03
    4 4 ['A005BIN'] 0.286726 ['A005BIN'] 0.53285736 0.562014 lr 0.03
    5 5 ['A00HBIN'] 0.28326982 ['A00HBIN'] 0.5305174 0.55573297 lr 0.03
    6 6 ['A007BIN'] 0.27790707 ['A007BIN'] 0.5284845 0.5487046 lr 0.03
    7 7 ['A034BIN'] 0.27982852 ['A034BIN'] 0.5263229 0.5530843 lr 0.03
    8 8 ['A00MBIN'] 0.27892584 ['A00MBIN'] 0.523264 0.5461206 lr 0.03
    9 9 ['A00GBIN'] 0.2749654 ['A00GBIN'] 0.5199225 0.54841495 lr 0.03


Line Recognition
================

Train a model with:

    ./ocropy ocroline-train -d http://storage.googleapis.com/lpr-ocr/uw3-dew-training.tgz \
            -t http://storage.googleapis.com/lpr-ocr/uw3-dew-testing.tgz

Models will be saved in the current directory.

You can view the training progress by connecting using VNC:

    xtightvncviewer :99

Models are saved in the current directory by default.

# Kubernetes

To run `ocropus3` on Kubernetes, you need to do the following:

- log into Google, set up Config.sh according to your project
- start up a Kubernetes cluster (`ku init`)
- submit your training job(s) (`kubectl apply -f ocroline-train.yaml`)

On GKE (Google Kubernetes Engine), you...

- write a job description in a YAML file
- use gs:// or http://storage.googleapis.com for your input shards
- save your models periodically to a Google storage bucket


```python
!cat Config.sh
```

    cluster=tmblearn
    zone=us-central1-f
    project=research-191823
    image=gcr.io/$project/ocropy
    cpu_machine=n1-standard-8
    cpu_nodes=3
    gpu_machine=n1-standard-16
    gpu_nodes=2



```python
!ku help
```

    init -- initialize the cluster
    daemonset -- start the NVIDIA daemonset
    status -- cluster status
    pods -- node list
    stats -- node stats
    build -- build the cloud image
    kill -- kill the cluster
    connect -- connect to a cluster
    forward -- connect to a cluster
    help -- display this help



```python
!ku status
```

    NAME      LOCATION       MASTER_VERSION  MASTER_IP       MACHINE_TYPE   NODE_VERSION  NUM_NODES  STATUS
    tmblearn  us-central1-f  1.9.7-gke.1     104.198.252.27  n1-standard-8  1.9.7-gke.1   5          RUNNING
    NAME          MACHINE_TYPE    DISK_SIZE_GB  NODE_VERSION
    default-pool  n1-standard-8   100           1.9.7-gke.1
    p100          n1-standard-16  100           1.9.7-gke.1



```python
!ku stats
```

    NAME      LOCATION       MASTER_VERSION  MASTER_IP       MACHINE_TYPE   NODE_VERSION  NUM_NODES  STATUS
    tmblearn  us-central1-f  1.9.7-gke.1     104.198.252.27  n1-standard-8  1.9.7-gke.1   5          RUNNING
    
    NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
    kubernetes   ClusterIP   10.27.240.1   <none>        443/TCP   1d
    
          1 ocroline-train Running
          1 ocroseg-train Running



```python
!cat ocroseg-train.yaml
```

    apiVersion: batch/v1
    kind: Job
    metadata:
      name: ocroseg-train
    spec:
      template:
        spec:
          containers:
          - name: ocroseg-train
            image: gcr.io/research-191823/ocropy
            workingDir: "/tmp"
            command: ["/usr/local/bin/runcmd"]
            args:
            - ocroseg-train
            - "-d"
            - "http://storage.googleapis.com/lpr-ocr/uw3-framed-lines.tgz"
            - "-o"
            - "ocroseg"
            resources:
              requests:
              limits:
                nvidia.com/gpu: "1"
                cpu: 12
                memory: "48000Mi"
          nodeSelector:
            cloud.google.com/gke-accelerator: nvidia-tesla-p100
          restartPolicy: Never
      backoffLimit: 4



```python
!gsutil ls gs://lpr-ocr/ | grep uw3
```

    gs://lpr-ocr/_uw3-patches.tgz
    gs://lpr-ocr/uw3-dew-testing.tgz
    gs://lpr-ocr/uw3-dew-training.tgz
    gs://lpr-ocr/uw3-framed-lines-test.tgz
    gs://lpr-ocr/uw3-framed-lines-train.tgz
    gs://lpr-ocr/uw3-framed-lines.tgz
    gs://lpr-ocr/uw3-framed-zones.tgz
    gs://lpr-ocr/uw3-lines-dew.tgz
    gs://lpr-ocr/uw3-lines.tgz
    gs://lpr-ocr/uw3-pages-test.tgz
    gs://lpr-ocr/uw3-pages-train.tgz
    gs://lpr-ocr/uw3-zones.tgz
    gs://lpr-ocr/_uw3-patches/
    gs://lpr-ocr/uw3-lines-old/



```python
!ku connect ocroline ls | head
```

    connecting to: ocroline-train-7fshb
    Miniconda2-latest-Linux-x86_64.sh  ol-000000665-010886.pt
    ol-000000005-239618.pt		   ol-000000670-007518.pt
    ol-000000010-182026.pt		   ol-000000675-007790.pt
    ol-000000015-119865.pt		   ol-000000680-008311.pt
    ol-000000020-094170.pt		   ol-000000685-007108.pt
    ol-000000025-079694.pt		   ol-000000690-010020.pt
    ol-000000030-048406.pt		   ol-000000695-006404.pt
    ol-000000035-058577.pt		   ol-000000700-007747.pt
    ol-000000040-039189.pt		   ol-000000705-007729.pt

