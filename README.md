Description
===========

Docker container for running OCRopus commands. This bundles up:

    - CUDA and PyTorch
    - A Python 2.7 Conda installation
    - The major modules from OCRopus3

PyTorch is currently at version 0.3.1, since OCRopus hasn't been converted yet
to PyTorch 0.4.

You probably should set a Jupyter password (see `jupyter_config/README`) and
a VNC password (see `vnc_config/README`).

To clone this repository, use the `--recursive` flag:

    git clone --recursive git@github.com:tmbdev/ocropus3-docker.git
    cd ocropus3-docker
    ./build

Segmentation
============

Train a model with:

    ./ocropy ocroseg-train -d http://storage.googleapis.com/lpr-ocr/uw3-lines-framed.tgz

Models will be saved in the current directory.

You can view the training progress by connecting using VNC:

    xtightvncviewer :99

Models are saved in the current directory by default.

Line Recognition
================

Train a model with:

    ./ocropy ocroline-train -d http://storage.googleapis.com/lpr-ocr/uw3-dew-training.tgz \
            -t http://storage.googleapis.com/lpr-ocr/uw3-dew-testing.tgz

Models will be saved in the current directory.

You can view the training progress by connecting using VNC:

    xtightvncviewer :99

Models are saved in the current directory by default.

