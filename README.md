
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
!./build
```

    
    
    
    
    Sending build context to Docker daemon  703.2MB
    Step 1/67 : FROM nvidia/cuda:9.0-base
     ---> f3a8582463d4
    Step 2/67 : MAINTAINER Tom Breuel <tmbdev@gmail.com>
     ---> Using cache
     ---> 488754540ac4
    Step 3/67 : ENV DEBIAN_FRONTEND noninteractive
     ---> Using cache
     ---> 01f341ae724c
    Step 4/67 : ENV DEBCONF_NONINTERACTIVE_SEEN true
     ---> Using cache
     ---> 18e13a972ea2
    Step 5/67 : RUN apt-get -y update
     ---> Using cache
     ---> 8adf79ab81c5
    Step 6/67 : RUN  apt-get -y install sudo lsb-release build-essential curl software-properties-common     && echo "deb http://packages.cloud.google.com/apt cloud-sdk-$(lsb_release -c -s) main"            >> /etc/apt/sources.list.d/google-cloud-sdk.list     && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key  add -     && apt-get update -y && apt-get dist-upgrade -y && apt-get -y install apt-utils     && apt-get -y install locales && locale-gen en_US.UTF-8 && dpkg-reconfigure locales
     ---> Using cache
     ---> 8c7bdef331e8
    Step 7/67 : RUN apt-get -y install wget tightvncserver tmux rxvt     xterm mlterm imagemagick firefox blackbox imagemagick     vim-gtk gnome-terminal i3 chromium-browser git mercurial lynx daemon
     ---> Using cache
     ---> b28fbd81a322
    Step 8/67 : RUN apt-get install -y nginx
     ---> Using cache
     ---> 9c2972ff8b53
    Step 9/67 : RUN apt-get install -y nginx-extras
     ---> Using cache
     ---> 3f3f54750547
    Step 10/67 : RUN apt-get install -y cadaver
     ---> Using cache
     ---> f17be7222090
    Step 11/67 : RUN cd /tmp     && wget --quiet -nd https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh     && bash ./Miniconda2-latest-Linux-x86_64.sh -b -p /opt/conda     && ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh     && ln -s /opt/conda/bin/conda /usr/bin/conda     && rm -f Miniconda*.sh
     ---> Using cache
     ---> 8e23892f426b
    Step 12/67 : RUN apt-get install -y redis-tools
     ---> Using cache
     ---> ace262171f29
    Step 13/67 : RUN conda install git
     ---> Running in 00f82e762c60
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - git
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        expat-2.2.5                |       he0dffb1_0         186 KB
        git-2.17.0                 |  pl526hb75a9fb_0        12.2 MB
        conda-4.5.4                |           py27_0         1.0 MB
        libcurl-7.60.0             |       h1ad7b7a_0         495 KB
        perl-5.26.2                |       h14c3975_0        15.9 MB
        libssh2-1.8.0              |       h9cfc8f7_4         243 KB
        ------------------------------------------------------------
                                               Total:        30.0 MB
    
    The following NEW packages will be INSTALLED:
    
        expat:   2.2.5-he0dffb1_0      
        git:     2.17.0-pl526hb75a9fb_0
        libcurl: 7.60.0-h1ad7b7a_0     
        libssh2: 1.8.0-h9cfc8f7_4      
        perl:    5.26.2-h14c3975_0     
    
    The following packages will be UPDATED:
    
        conda:   4.5.1-py27_0           --> 4.5.4-py27_0
    
    Proceed ([y]/n)? 
    expat 2.2.5########## | 100% [0m[91m
    git 2.17.0########## | 100% [0m[91m[91m[91m[91m[91m
    conda 4.5.4########## | 100% [0m[91m[91m[91m
    libcurl 7.60.0########## | 100% [0m[91m[91m
    perl 5.26.2########## | 100% [0m[91m[91m[91m[91m[91m
    libssh2 1.8.0########## | 100% [0m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 00f82e762c60
     ---> 63136a830e7d
    Step 14/67 : RUN conda install numpy
     ---> Running in 3d482679c7a5
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - numpy
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        numpy-base-1.14.3          |   py27hdbf6ddf_2         4.1 MB
        mkl_random-1.0.1           |   py27h629b387_0         361 KB
        mkl-2018.0.3               |                1       198.7 MB
        intel-openmp-2018.0.3      |                0         705 KB
        numpy-1.14.3               |   py27hcd700cb_2          42 KB
        mkl_fft-1.0.1              |   py27h3010b51_0         137 KB
        blas-1.0                   |              mkl           6 KB
        libgfortran-ng-7.2.0       |       hdf63c60_3         1.2 MB
        ------------------------------------------------------------
                                               Total:       205.2 MB
    
    The following NEW packages will be INSTALLED:
    
        blas:           1.0-mkl              
        intel-openmp:   2018.0.3-0           
        libgfortran-ng: 7.2.0-hdf63c60_3     
        mkl:            2018.0.3-1           
        mkl_fft:        1.0.1-py27h3010b51_0 
        mkl_random:     1.0.1-py27h629b387_0 
        numpy:          1.14.3-py27hcd700cb_2
        numpy-base:     1.14.3-py27hdbf6ddf_2
    
    Proceed ([y]/n)? 
    numpy-base-1.14.3    |  4.1 MB | ########## | 100% [0m[91m[91m[91m[91m[91m
    mkl_random-1.0.1     |  361 KB | ########## | 100% [0m[91m
    mkl-2018.0.3         | 198.7 MB | ########## | 100% [0m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m
    intel-openmp-2018.0. |  705 KB | ########## | 100% [0m[91m[91m
    numpy-1.14.3         |   42 KB | ########## | 100% [0m[91m
    mkl_fft-1.0.1        |  137 KB | ########## | 100% [0m[91m
    blas-1.0             |    6 KB | ########## | 100% [0m[91m
    libgfortran-ng-7.2.0 |  1.2 MB | ########## | 100% [0m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 3d482679c7a5
     ---> a40f857878b0
    Step 15/67 : RUN conda install scipy
     ---> Running in 85ad8ad51d1a
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - scipy
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        scipy-1.1.0                |   py27hfc37229_0        17.9 MB
    
    The following NEW packages will be INSTALLED:
    
        scipy: 1.1.0-py27hfc37229_0
    
    Proceed ([y]/n)? 
    scipy-1.1.0          | 17.9 MB | ########## | 100% [0m[91m[91m[91m[91m[91m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 85ad8ad51d1a
     ---> 4aebcd089170
    Step 16/67 : RUN conda install msgpack
     ---> Running in 2cff04a900f3
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - msgpack
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        msgpack-0.2.3              |           py27_0         100 KB
    
    The following NEW packages will be INSTALLED:
    
        msgpack: 0.2.3-py27_0
    
    Proceed ([y]/n)? 
    msgpack-0.2.3        |  100 KB | ########## | 100% [0m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 2cff04a900f3
     ---> 07adbe16f177
    Step 17/67 : RUN conda install simplejson
     ---> Running in e825dcc82f91
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - simplejson
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        simplejson-3.15.0          |   py27h14c3975_0         101 KB
    
    The following NEW packages will be INSTALLED:
    
        simplejson: 3.15.0-py27h14c3975_0
    
    Proceed ([y]/n)? 
    simplejson-3.15.0    |  101 KB | ########## | 100% [0m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container e825dcc82f91
     ---> d3e6df3cad46
    Step 18/67 : RUN conda install pyzmq
     ---> Running in fcaf91cf6f72
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - pyzmq
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        libsodium-1.0.16           |       h1bed415_0         302 KB
        pyzmq-17.0.0               |   py27h14c3975_1         440 KB
        zeromq-4.2.5               |       h439df22_0         567 KB
        ------------------------------------------------------------
                                               Total:         1.3 MB
    
    The following NEW packages will be INSTALLED:
    
        libsodium: 1.0.16-h1bed415_0    
        pyzmq:     17.0.0-py27h14c3975_1
        zeromq:    4.2.5-h439df22_0     
    
    Proceed ([y]/n)? 
    libsodium-1.0.16     |  302 KB | ########## | 100% [0m[91m[91m[91m
    pyzmq-17.0.0         |  440 KB | ########## | 100% [0m[91m[91m
    zeromq-4.2.5         |  567 KB | ########## | 100% [0m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container fcaf91cf6f72
     ---> 0f145cabe572
    Step 19/67 : RUN conda install jupyter
     ---> Running in 457d85058cc5
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - jupyter
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        dbus-1.13.2                |       h714fa37_1         554 KB
        webencodings-0.5.1         |   py27hff10b21_1          19 KB
        terminado-0.8.1            |           py27_1          20 KB
        backports-1.0              |   py27h63c9359_1           3 KB
        ipykernel-4.8.2            |           py27_0         143 KB
        nbformat-4.4.0             |   py27hed7f2b2_0         136 KB
        ipython-5.7.0              |           py27_0         1.0 MB
        xz-5.2.4                   |       h14c3975_4         366 KB
        sip-4.19.8                 |   py27hf484d3e_0         291 KB
        backports_abc-0.5          |   py27h7b3c97b_0          12 KB
        freetype-2.8               |       hab7d2ae_1         804 KB
        qtconsole-4.3.1            |   py27hc444b0d_0         147 KB
        jpeg-9b                    |       h024ee3a_2         248 KB
        markupsafe-1.0             |   py27h97b2822_1          24 KB
        backports.shutil_get_terminal_size-1.0.0|   py27h5bc021e_2           8 KB
        gst-plugins-base-1.14.0    |       hbbd80ab_1         6.3 MB
        python-dateutil-2.7.3      |           py27_0         258 KB
        pcre-8.42                  |       h439df22_0         251 KB
        testpath-0.3.1             |   py27hc38d2c4_0          89 KB
        pexpect-4.6.0              |           py27_0          74 KB
        scandir-1.7                |   py27h14c3975_0          27 KB
        jsonschema-2.6.0           |   py27h7ed5aa4_0          61 KB
        notebook-5.5.0             |           py27_0         7.0 MB
        libxcb-1.13                |       h1bed415_1         502 KB
        html5lib-1.0.1             |   py27h5233db4_0         188 KB
        widgetsnbextension-3.2.1   |           py27_0         1.7 MB
        pickleshare-0.7.4          |   py27h09770e1_0          11 KB
        libxml2-2.9.8              |       h26e45fe_1         2.0 MB
        pandocfilters-1.4.2        |   py27h428e1e5_1          12 KB
        singledispatch-3.4.0.3     |   py27h9bcb476_0          15 KB
        bleach-2.1.3               |           py27_0          32 KB
        jupyter_core-4.4.0         |   py27h345911c_0          60 KB
        mistune-0.8.3              |   py27h14c3975_1         266 KB
        decorator-4.3.0            |           py27_0          15 KB
        send2trash-1.5.0           |           py27_0          16 KB
        gstreamer-1.14.0           |       hb453b48_1         3.8 MB
        ipython_genutils-0.2.0     |   py27h89fb69b_0          38 KB
        fontconfig-2.12.6          |       h49f89f6_0         283 KB
        pyqt-5.9.2                 |   py27h751905a_0         5.7 MB
        icu-58.2                   |       h9c2bf20_1        22.5 MB
        pandoc-1.19.2.1            |       hea2e7c5_1        17.8 MB
        pathlib2-2.3.2             |           py27_0          31 KB
        simplegeneric-0.8.1        |           py27_2           9 KB
        qt-5.9.5                   |       h7e424d6_0        84.9 MB
        tornado-5.0.2              |           py27_0         620 KB
        entrypoints-0.2.3          |   py27h502b47d_2           9 KB
        jupyter_console-5.2.0      |   py27hc6bee7e_1          34 KB
        jupyter-1.0.0              |           py27_4           5 KB
        functools32-3.2.3.2        |   py27h4ead58f_1          22 KB
        ptyprocess-0.5.2           |   py27h4ccb14c_0          22 KB
        prompt_toolkit-1.0.15      |   py27h1b593e1_0         333 KB
        nbconvert-5.3.1            |   py27he041f76_0         395 KB
        jupyter_client-5.2.3       |           py27_0         122 KB
        libpng-1.6.34              |       hb9fc6fc_0         334 KB
        pygments-2.2.0             |   py27h4a8b6f5_0         1.3 MB
        wcwidth-0.1.7              |   py27h9e3e1ab_0          25 KB
        gmp-6.1.2                  |       h6c8ec71_1         744 KB
        ipywidgets-7.2.1           |           py27_0         141 KB
        jinja2-2.10                |   py27h4114e70_0         177 KB
        traitlets-4.3.2            |   py27hd6ce930_0         126 KB
        glib-2.56.1                |       h000015b_0         5.0 MB
        configparser-3.5.0         |   py27h5117587_0          35 KB
        ------------------------------------------------------------
                                               Total:       167.0 MB
    
    The following NEW packages will be INSTALLED:
    
        backports:                          1.0-py27h63c9359_1    
        backports.shutil_get_terminal_size: 1.0.0-py27h5bc021e_2  
        backports_abc:                      0.5-py27h7b3c97b_0    
        bleach:                             2.1.3-py27_0          
        configparser:                       3.5.0-py27h5117587_0  
        dbus:                               1.13.2-h714fa37_1     
        decorator:                          4.3.0-py27_0          
        entrypoints:                        0.2.3-py27h502b47d_2  
        fontconfig:                         2.12.6-h49f89f6_0     
        freetype:                           2.8-hab7d2ae_1        
        functools32:                        3.2.3.2-py27h4ead58f_1
        glib:                               2.56.1-h000015b_0     
        gmp:                                6.1.2-h6c8ec71_1      
        gst-plugins-base:                   1.14.0-hbbd80ab_1     
        gstreamer:                          1.14.0-hb453b48_1     
        html5lib:                           1.0.1-py27h5233db4_0  
        icu:                                58.2-h9c2bf20_1       
        ipykernel:                          4.8.2-py27_0          
        ipython:                            5.7.0-py27_0          
        ipython_genutils:                   0.2.0-py27h89fb69b_0  
        ipywidgets:                         7.2.1-py27_0          
        jinja2:                             2.10-py27h4114e70_0   
        jpeg:                               9b-h024ee3a_2         
        jsonschema:                         2.6.0-py27h7ed5aa4_0  
        jupyter:                            1.0.0-py27_4          
        jupyter_client:                     5.2.3-py27_0          
        jupyter_console:                    5.2.0-py27hc6bee7e_1  
        jupyter_core:                       4.4.0-py27h345911c_0  
        libpng:                             1.6.34-hb9fc6fc_0     
        libxcb:                             1.13-h1bed415_1       
        libxml2:                            2.9.8-h26e45fe_1      
        markupsafe:                         1.0-py27h97b2822_1    
        mistune:                            0.8.3-py27h14c3975_1  
        nbconvert:                          5.3.1-py27he041f76_0  
        nbformat:                           4.4.0-py27hed7f2b2_0  
        notebook:                           5.5.0-py27_0          
        pandoc:                             1.19.2.1-hea2e7c5_1   
        pandocfilters:                      1.4.2-py27h428e1e5_1  
        pathlib2:                           2.3.2-py27_0          
        pcre:                               8.42-h439df22_0       
        pexpect:                            4.6.0-py27_0          
        pickleshare:                        0.7.4-py27h09770e1_0  
        prompt_toolkit:                     1.0.15-py27h1b593e1_0 
        ptyprocess:                         0.5.2-py27h4ccb14c_0  
        pygments:                           2.2.0-py27h4a8b6f5_0  
        pyqt:                               5.9.2-py27h751905a_0  
        python-dateutil:                    2.7.3-py27_0          
        qt:                                 5.9.5-h7e424d6_0      
        qtconsole:                          4.3.1-py27hc444b0d_0  
        scandir:                            1.7-py27h14c3975_0    
        send2trash:                         1.5.0-py27_0          
        simplegeneric:                      0.8.1-py27_2          
        singledispatch:                     3.4.0.3-py27h9bcb476_0
        sip:                                4.19.8-py27hf484d3e_0 
        terminado:                          0.8.1-py27_1          
        testpath:                           0.3.1-py27hc38d2c4_0  
        tornado:                            5.0.2-py27_0          
        traitlets:                          4.3.2-py27hd6ce930_0  
        wcwidth:                            0.1.7-py27h9e3e1ab_0  
        webencodings:                       0.5.1-py27hff10b21_1  
        widgetsnbextension:                 3.2.1-py27_0          
        xz:                                 5.2.4-h14c3975_4      
    
    Proceed ([y]/n)? 
    dbus-1.13.2          |  554 KB | ########## | 100% [0m[91m[91m
    webencodings-0.5.1   |   19 KB | ########## | 100% [0m[91m
    terminado-0.8.1      |   20 KB | ########## | 100% [0m[91m
    backports-1.0        |    3 KB | ########## | 100% [0m[91m
    ipykernel-4.8.2      |  143 KB | ########## | 100% [0m[91m[91m
    nbformat-4.4.0       |  136 KB | ########## | 100% [0m[91m
    ipython-5.7.0        |  1.0 MB | ########## | 100% [0m[91m[91m[91m
    xz-5.2.4             |  366 KB | ########## | 100% [0m[91m[91m
    sip-4.19.8           |  291 KB | ########## | 100% [0m[91m
    backports_abc-0.5    |   12 KB | ########## | 100% [0m[91m
    freetype-2.8         |  804 KB | ########## | 100% [0m[91m[91m
    qtconsole-4.3.1      |  147 KB | ########## | 100% [0m[91m
    jpeg-9b              |  248 KB | ########## | 100% [0m[91m
    markupsafe-1.0       |   24 KB | ########## | 100% [0m[91m
    backports.shutil_get |    8 KB | ########## | 100% [0m[91m
    gst-plugins-base-1.1 |  6.3 MB | ########## | 100% [0m[91m[91m[91m
    python-dateutil-2.7. |  258 KB | ########## | 100% [0m[91m
    pcre-8.42            |  251 KB | ########## | 100% [0m[91m
    testpath-0.3.1       |   89 KB | ########## | 100% [0m[91m
    pexpect-4.6.0        |   74 KB | ########## | 100% [0m[91m
    scandir-1.7          |   27 KB | ########## | 100% [0m[91m
    jsonschema-2.6.0     |   61 KB | ########## | 100% [0m[91m
    notebook-5.5.0       |  7.0 MB | ########## | 100% [0m[91m[91m[91m[91m[91m
    libxcb-1.13          |  502 KB | ########## | 100% [0m[91m[91m
    html5lib-1.0.1       |  188 KB | ########## | 100% [0m[91m
    widgetsnbextension-3 |  1.7 MB | ########## | 100% [0m[91m[91m
    pickleshare-0.7.4    |   11 KB | ########## | 100% [0m[91m
    libxml2-2.9.8        |  2.0 MB | ########## | 100% [0m[91m[91m[91m
    pandocfilters-1.4.2  |   12 KB | ########## | 100% [0m[91m
    singledispatch-3.4.0 |   15 KB | ########## | 100% [0m[91m
    bleach-2.1.3         |   32 KB | ########## | 100% [0m[91m
    jupyter_core-4.4.0   |   60 KB | ########## | 100% [0m[91m
    mistune-0.8.3        |  266 KB | ########## | 100% [0m[91m[91m
    decorator-4.3.0      |   15 KB | ########## | 100% [0m[91m
    send2trash-1.5.0     |   16 KB | ########## | 100% [0m[91m
    gstreamer-1.14.0     |  3.8 MB | ########## | 100% [0m[91m[91m[91m
    ipython_genutils-0.2 |   38 KB | ########## | 100% [0m[91m
    fontconfig-2.12.6    |  283 KB | ########## | 100% [0m[91m
    pyqt-5.9.2           |  5.7 MB | ########## | 100% [0m[91m[91m[91m[91m
    icu-58.2             | 22.5 MB | ########## | 100% [0m[91m[91m[91m[91m
    pandoc-1.19.2.1      | 17.8 MB | ########## | 100% [0m[91m[91m[91m[91m
    pathlib2-2.3.2       |   31 KB | ########## | 100% [0m[91m
    simplegeneric-0.8.1  |    9 KB | ########## | 100% [0m[91m
    qt-5.9.5             | 84.9 MB | ########## | 100% [0m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m
    tornado-5.0.2        |  620 KB | ########## | 100% [0m[91m[91m
    entrypoints-0.2.3    |    9 KB | ########## | 100% [0m[91m
    jupyter_console-5.2. |   34 KB | ########## | 100% [0m[91m
    jupyter-1.0.0        |    5 KB | ########## | 100% [0m[91m
    functools32-3.2.3.2  |   22 KB | ########## | 100% [0m[91m
    ptyprocess-0.5.2     |   22 KB | ########## | 100% [0m[91m
    prompt_toolkit-1.0.1 |  333 KB | ########## | 100% [0m[91m[91m
    nbconvert-5.3.1      |  395 KB | ########## | 100% [0m[91m[91m
    jupyter_client-5.2.3 |  122 KB | ########## | 100% [0m[91m
    libpng-1.6.34        |  334 KB | ########## | 100% [0m[91m
    pygments-2.2.0       |  1.3 MB | ########## | 100% [0m[91m[91m[91m
    wcwidth-0.1.7        |   25 KB | ########## | 100% [0m[91m
    gmp-6.1.2            |  744 KB | ########## | 100% [0m[91m[91m
    ipywidgets-7.2.1     |  141 KB | ########## | 100% [0m[91m
    jinja2-2.10          |  177 KB | ########## | 100% [0m[91m
    traitlets-4.3.2      |  126 KB | ########## | 100% [0m[91m
    glib-2.56.1          |  5.0 MB | ########## | 100% [0m[91m[91m[91m[91m
    configparser-3.5.0   |   35 KB | ########## | 100% [0m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 457d85058cc5
     ---> 7b486c693a0c
    Step 20/67 : RUN conda install scikit-image
     ---> Running in 3e72ad8c8e8f
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - scikit-image
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        subprocess32-3.5.1         |   py27h14c3975_0          40 KB
        dask-0.17.5                |           py27_0           3 KB
        pyyaml-3.12                |   py27h2d70dd7_1         159 KB
        distributed-1.21.8         |           py27_0         748 KB
        pyparsing-2.2.0            |   py27hf1513f8_1          93 KB
        bokeh-0.12.16              |           py27_0         4.2 MB
        tblib-1.3.2                |   py27h51fe5ba_0          15 KB
        toolz-0.9.0                |           py27_0          90 KB
        packaging-17.1             |           py27_0          32 KB
        matplotlib-2.2.2           |   py27h0e671d2_1         6.5 MB
        olefile-0.45.1             |           py27_0          47 KB
        scikit-image-0.13.1        |   py27h14c3975_1        23.2 MB
        pandas-0.23.0              |   py27h637b7d7_0        11.8 MB
        psutil-5.4.5               |   py27h14c3975_0         297 KB
        locket-0.2.0               |   py27h73929a2_1           8 KB
        heapdict-1.0.0             |           py27_2           8 KB
        sortedcontainers-2.0.3     |           py27_0          41 KB
        zict-0.1.3                 |   py27h12c336c_0          18 KB
        cytoolz-0.9.0.1            |   py27h14c3975_0         410 KB
        backports.functools_lru_cache-1.5|           py27_1           9 KB
        pytz-2018.4                |           py27_0         211 KB
        kiwisolver-1.0.1           |   py27hc15e7b5_0          86 KB
        dask-core-0.17.5           |           py27_0         1.0 MB
        cloudpickle-0.5.3          |           py27_0          25 KB
        msgpack-python-0.5.6       |   py27h6bb024c_0          95 KB
        networkx-2.1               |           py27_0         1.8 MB
        partd-0.3.8                |   py27h4e55004_0          30 KB
        libtiff-4.0.9              |       he85c1e1_1         566 KB
        pillow-5.1.0               |   py27h3deb7b8_0         577 KB
        cycler-0.10.0              |   py27hc7354d3_0          13 KB
        pywavelets-0.5.2           |   py27hecda097_0         4.0 MB
        click-6.7                  |   py27h4225b90_0         103 KB
        imageio-2.3.0              |           py27_0         3.3 MB
        ------------------------------------------------------------
                                               Total:        59.4 MB
    
    The following NEW packages will be INSTALLED:
    
        backports.functools_lru_cache: 1.5-py27_1            
        bokeh:                         0.12.16-py27_0        
        click:                         6.7-py27h4225b90_0    
        cloudpickle:                   0.5.3-py27_0          
        cycler:                        0.10.0-py27hc7354d3_0 
        cytoolz:                       0.9.0.1-py27h14c3975_0
        dask:                          0.17.5-py27_0         
        dask-core:                     0.17.5-py27_0         
        distributed:                   1.21.8-py27_0         
        heapdict:                      1.0.0-py27_2          
        imageio:                       2.3.0-py27_0          
        kiwisolver:                    1.0.1-py27hc15e7b5_0  
        libtiff:                       4.0.9-he85c1e1_1      
        locket:                        0.2.0-py27h73929a2_1  
        matplotlib:                    2.2.2-py27h0e671d2_1  
        msgpack-python:                0.5.6-py27h6bb024c_0  
        networkx:                      2.1-py27_0            
        olefile:                       0.45.1-py27_0         
        packaging:                     17.1-py27_0           
        pandas:                        0.23.0-py27h637b7d7_0 
        partd:                         0.3.8-py27h4e55004_0  
        pillow:                        5.1.0-py27h3deb7b8_0  
        psutil:                        5.4.5-py27h14c3975_0  
        pyparsing:                     2.2.0-py27hf1513f8_1  
        pytz:                          2018.4-py27_0         
        pywavelets:                    0.5.2-py27hecda097_0  
        pyyaml:                        3.12-py27h2d70dd7_1   
        scikit-image:                  0.13.1-py27h14c3975_1 
        sortedcontainers:              2.0.3-py27_0          
        subprocess32:                  3.5.1-py27h14c3975_0  
        tblib:                         1.3.2-py27h51fe5ba_0  
        toolz:                         0.9.0-py27_0          
        zict:                          0.1.3-py27h12c336c_0  
    
    Proceed ([y]/n)? 
    subprocess32-3.5.1   |   40 KB | ########## | 100% [0m[91m
    dask-0.17.5          |    3 KB | ########## | 100% [0m[91m
    pyyaml-3.12          |  159 KB | ########## | 100% [0m[91m
    distributed-1.21.8   |  748 KB | ########## | 100% [0m[91m[91m[91m
    pyparsing-2.2.0      |   93 KB | ########## | 100% [0m[91m
    bokeh-0.12.16        |  4.2 MB | ########## | 100% [0m[91m[91m[91m[91m
    tblib-1.3.2          |   15 KB | ########## | 100% [0m[91m
    toolz-0.9.0          |   90 KB | ########## | 100% [0m[91m
    packaging-17.1       |   32 KB | ########## | 100% [0m[91m
    matplotlib-2.2.2     |  6.5 MB | ########## | 100% [0m[91m[91m[91m[91m
    olefile-0.45.1       |   47 KB | ########## | 100% [0m[91m
    scikit-image-0.13.1  | 23.2 MB | ########## | 100% [0m[91m[91m[91m[91m[91m
    pandas-0.23.0        | 11.8 MB | ########## | 100% [0m[91m[91m[91m[91m
    psutil-5.4.5         |  297 KB | ########## | 100% [0m[91m
    locket-0.2.0         |    8 KB | ########## | 100% [0m[91m
    heapdict-1.0.0       |    8 KB | ########## | 100% [0m[91m
    sortedcontainers-2.0 |   41 KB | ########## | 100% [0m[91m
    zict-0.1.3           |   18 KB | ########## | 100% [0m[91m
    cytoolz-0.9.0.1      |  410 KB | ########## | 100% [0m[91m[91m
    backports.functools_ |    9 KB | ########## | 100% [0m[91m
    pytz-2018.4          |  211 KB | ########## | 100% [0m[91m[91m
    kiwisolver-1.0.1     |   86 KB | ########## | 100% [0m[91m
    dask-core-0.17.5     |  1.0 MB | ########## | 100% [0m[91m[91m[91m
    cloudpickle-0.5.3    |   25 KB | ########## | 100% [0m[91m
    msgpack-python-0.5.6 |   95 KB | ########## | 100% [0m[91m
    networkx-2.1         |  1.8 MB | ########## | 100% [0m[91m[91m[91m
    partd-0.3.8          |   30 KB | ########## | 100% [0m[91m
    libtiff-4.0.9        |  566 KB | ########## | 100% [0m[91m[91m
    pillow-5.1.0         |  577 KB | ########## | 100% [0m[91m[91m
    cycler-0.10.0        |   13 KB | ########## | 100% [0m[91m
    pywavelets-0.5.2     |  4.0 MB | ########## | 100% [0m[91m[91m[91m
    click-6.7            |  103 KB | ########## | 100% [0m[91m
    imageio-2.3.0        |  3.3 MB | ########## | 100% [0m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 3e72ad8c8e8f
     ---> e90431492ad6
    Step 21/67 : RUN conda install scikit-learn
     ---> Running in e2dab7bfb01d
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - scikit-learn
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        scikit-learn-0.19.1        |   py27h445a80a_0         5.3 MB
    
    The following NEW packages will be INSTALLED:
    
        scikit-learn: 0.19.1-py27h445a80a_0
    
    Proceed ([y]/n)? 
    scikit-learn-0.19.1  |  5.3 MB | ########## | 100% [0m[91m[91m[91m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container e2dab7bfb01d
     ---> ca612e695623
    Step 22/67 : RUN conda install redis
     ---> Running in 291dc0af760b
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - redis
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        redis-4.0.9                |       h14c3975_0         7.3 MB
    
    The following NEW packages will be INSTALLED:
    
        redis: 4.0.9-h14c3975_0
    
    Proceed ([y]/n)? 
    redis-4.0.9          |  7.3 MB | ########## | 100% [0m[91m[91m[91m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 291dc0af760b
     ---> 15b3723ddf74
    Step 23/67 : RUN conda install Pillow
     ---> Running in df9e53e7178d
    Solving environment: ...working... done
    
    # All requested packages already installed.
    
    Removing intermediate container df9e53e7178d
     ---> 05bdf693abb9
    Step 24/67 : RUN conda install pytorch=0.3.1 torchvision cuda90 -c pytorch
     ---> Running in d29415dc80e9
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - cuda90
        - pytorch=0.3.1
        - torchvision
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        cuda90-1.0                 |       h6433d27_0           3 KB  pytorch
        torchvision-0.2.0          |   py27hfb27419_1         102 KB  pytorch
        pytorch-0.3.1              |py27_cuda9.0.176_cudnn7.0.5_2       486.9 MB  pytorch
        ------------------------------------------------------------
                                               Total:       487.0 MB
    
    The following NEW packages will be INSTALLED:
    
        cuda90:      1.0-h6433d27_0                      pytorch
        pytorch:     0.3.1-py27_cuda9.0.176_cudnn7.0.5_2 pytorch [cuda90]
        torchvision: 0.2.0-py27hfb27419_1                pytorch
    
    Proceed ([y]/n)? 
    cuda90-1.0           |    3 KB | ########## | 100% [0m[91m[91m
    torchvision-0.2.0    |  102 KB | ########## | 100% [0m[91m[91m[91m
    pytorch-0.3.1        | 486.9 MB | ########## | 100% [0m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container d29415dc80e9
     ---> 02bc7bf3f928
    Step 25/67 : RUN conda install msgpack
     ---> Running in ce3c9931064c
    Solving environment: ...working... done
    
    # All requested packages already installed.
    
    Removing intermediate container ce3c9931064c
     ---> 155c118ce605
    Step 26/67 : RUN conda install -c conda-forge google-cloud-storage
     ---> Running in 8db4786073a6
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - google-cloud-storage
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        openssl-1.0.2o             |                0         3.5 MB  conda-forge
        google-resumable-media-0.3.1|             py_0          28 KB  conda-forge
        google-cloud-core-0.28.1   |             py_0          23 KB  conda-forge
        pyasn1-0.4.3               |             py_0          46 KB  conda-forge
        certifi-2018.4.16          |           py27_0         142 KB  conda-forge
        googleapis-common-protos-1.5.3|           py27_0          51 KB  conda-forge
        pyasn1-modules-0.2.1       |             py_0          33 KB  conda-forge
        google-auth-1.5.0          |             py_0          39 KB  conda-forge
        protobuf-3.5.2             |           py27_0         1.3 MB  conda-forge
        ca-certificates-2018.4.16  |                0         139 KB  conda-forge
        libprotobuf-3.5.2          |                0         4.5 MB  conda-forge
        google-api-core-0.1.4      |             py_0          36 KB  conda-forge
        google-cloud-storage-1.10.0|             py_0          40 KB  conda-forge
        cachetools-2.0.1           |             py_0          10 KB  conda-forge
        conda-4.5.4                |           py27_0         629 KB  conda-forge
        rsa-3.4.2                  |           py27_0          50 KB  conda-forge
        ------------------------------------------------------------
                                               Total:        10.6 MB
    
    The following NEW packages will be INSTALLED:
    
        cachetools:               2.0.1-py_0        conda-forge
        google-api-core:          0.1.4-py_0        conda-forge
        google-auth:              1.5.0-py_0        conda-forge
        google-cloud-core:        0.28.1-py_0       conda-forge
        google-cloud-storage:     1.10.0-py_0       conda-forge
        google-resumable-media:   0.3.1-py_0        conda-forge
        googleapis-common-protos: 1.5.3-py27_0      conda-forge
        libprotobuf:              3.5.2-0           conda-forge
        protobuf:                 3.5.2-py27_0      conda-forge
        pyasn1:                   0.4.3-py_0        conda-forge
        pyasn1-modules:           0.2.1-py_0        conda-forge
        rsa:                      3.4.2-py27_0      conda-forge
    
    The following packages will be UPDATED:
    
        ca-certificates:          2018.03.07-0                  --> 2018.4.16-0      conda-forge
        certifi:                  2018.4.16-py27_0              --> 2018.4.16-py27_0 conda-forge
        conda:                    4.5.4-py27_0                  --> 4.5.4-py27_0     conda-forge
        openssl:                  1.0.2o-h20670df_0             --> 1.0.2o-0         conda-forge
    
    Proceed ([y]/n)? 
    openssl-1.0.2o       |  3.5 MB | ########## | 100% [0m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m[91m
    google-resumable-med |   28 KB | ########## | 100% [0m[91m[91m
    google-cloud-core-0. |   23 KB | ########## | 100% [0m[91m[91m
    pyasn1-0.4.3         |   46 KB | ########## | 100% [0m[91m[91m
    certifi-2018.4.16    |  142 KB | ########## | 100% [0m[91m[91m
    googleapis-common-pr |   51 KB | ########## | 100% [0m[91m[91m
    pyasn1-modules-0.2.1 |   33 KB | ########## | 100% [0m[91m[91m
    google-auth-1.5.0    |   39 KB | ########## | 100% [0m[91m[91m
    protobuf-3.5.2       |  1.3 MB | ########## | 100% [0m[91m[91m[91m[91m[91m[91m
    ca-certificates-2018 |  139 KB | ########## | 100% [0m[91m[91m
    libprotobuf-3.5.2    |  4.5 MB | ########## | 100% [0m[91m[91m[91m[91m[91m[91m[91m[91m[91m
    google-api-core-0.1. |   36 KB | ########## | 100% [0m[91m[91m
    google-cloud-storage |   40 KB | ########## | 100% [0m[91m
    cachetools-2.0.1     |   10 KB | ########## | 100% [0m[91m[91m
    conda-4.5.4          |  629 KB | ########## | 100% [0m[91m[91m[91m
    rsa-3.4.2            |   50 KB | ########## | 100% [0m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 8db4786073a6
     ---> 5bd87ea741d2
    Step 27/67 : RUN conda install cython
     ---> Running in 7f1fe3575555
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - cython
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        conda-4.5.4                |           py27_0         1.0 MB
        cython-0.28.3              |   py27h14c3975_0         3.3 MB
        certifi-2018.4.16          |           py27_0         142 KB
        ------------------------------------------------------------
                                               Total:         4.4 MB
    
    The following NEW packages will be INSTALLED:
    
        cython:          0.28.3-py27h14c3975_0            
    
    The following packages will be UPDATED:
    
        certifi:         2018.4.16-py27_0      conda-forge --> 2018.4.16-py27_0 
        conda:           4.5.4-py27_0          conda-forge --> 4.5.4-py27_0     
        openssl:         1.0.2o-0              conda-forge --> 1.0.2o-h20670df_0
    
    The following packages will be DOWNGRADED:
    
        ca-certificates: 2018.4.16-0           conda-forge --> 2018.03.07-0     
    
    Proceed ([y]/n)? 
    conda-4.5.4          |  1.0 MB | ########## | 100% [0m[91m[91m[91m
    cython-0.28.3        |  3.3 MB | ########## | 100% [0m[91m[91m[91m
    certifi-2018.4.16    |  142 KB | ########## | 100% [0m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 7f1fe3575555
     ---> 42e0ea3fa178
    Step 28/67 : RUN conda install -c auto editdistance
     ---> Running in 1d6652073ce6
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - editdistance
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        editdistance-0.2           |           py27_0         174 KB  auto
    
    The following NEW packages will be INSTALLED:
    
        editdistance: 0.2-py27_0 auto
    
    Proceed ([y]/n)? 
    editdistance-0.2     |  174 KB | ########## | 100% [0m[91m[91m[91m[91m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 1d6652073ce6
     ---> 411439573253
    Step 29/67 : RUN conda install pip     && ln -s /opt/conda/bin/pip /usr/bin/pip
     ---> Running in 95fda1b19a5e
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - pip
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        pip-10.0.1                 |           py27_0         1.7 MB
    
    The following packages will be UPDATED:
    
        pip: 9.0.3-py27_0 --> 10.0.1-py27_0
    
    Proceed ([y]/n)? 
    pip-10.0.1           |  1.7 MB | ########## | 100% [0m[91m[91m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 95fda1b19a5e
     ---> 52917697a9dd
    Step 30/67 : RUN conda install setuptools
     ---> Running in 2ec945118a95
    Solving environment: ...working... done
    
    ## Package Plan ##
    
      environment location: /opt/conda
    
      added / updated specs: 
        - setuptools
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        setuptools-39.2.0          |           py27_0         583 KB
    
    The following packages will be UPDATED:
    
        setuptools: 39.0.1-py27_0 --> 39.2.0-py27_0
    
    Proceed ([y]/n)? 
    setuptools-39.2.0    |  583 KB | ########## | 100% [0m[91m[91m[91m
    [0m
    Downloading and Extracting Packages
    Preparing transaction: ...working... done
    Verifying transaction: ...working... done
    Executing transaction: ...working... done
    Removing intermediate container 2ec945118a95
     ---> b9afb533016d
    Step 31/67 : ENV PATH /opt/conda/bin:/usr/local/nvidia/bin:/usr/local/cuda/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
     ---> Running in 32cd4bba8c38
    Removing intermediate container 32cd4bba8c38
     ---> 36df4d285e4b
    Step 32/67 : COPY dlinputs /tmp/dlinputs
     ---> e819cb766cac
    Step 33/67 : RUN cd /tmp/dlinputs && python setup.py install && rm -rf /tmp/dlinputs
     ---> Running in 9035b52b4bca
    running install
    running build
    running build_py
    creating build
    creating build/lib
    creating build/lib/dlinputs
    copying dlinputs/loader.py -> build/lib/dlinputs
    copying dlinputs/zcom.py -> build/lib/dlinputs
    copying dlinputs/sequence.py -> build/lib/dlinputs
    copying dlinputs/utils.py -> build/lib/dlinputs
    copying dlinputs/aggregator.py -> build/lib/dlinputs
    copying dlinputs/gopen.py -> build/lib/dlinputs
    copying dlinputs/tarrecords.py -> build/lib/dlinputs
    copying dlinputs/filters.py -> build/lib/dlinputs
    copying dlinputs/parallel.py -> build/lib/dlinputs
    copying dlinputs/__init__.py -> build/lib/dlinputs
    copying dlinputs/sqlitedb.py -> build/lib/dlinputs
    copying dlinputs/sources.py -> build/lib/dlinputs
    copying dlinputs/paths.py -> build/lib/dlinputs
    copying dlinputs/improc.py -> build/lib/dlinputs
    copying dlinputs/loadable.py -> build/lib/dlinputs
    running build_scripts
    creating build/scripts-2.7
    copying and adjusting tarshards -> build/scripts-2.7
    copying and adjusting show-input -> build/scripts-2.7
    copying and adjusting training-test-split -> build/scripts-2.7
    copying and adjusting lsmodel -> build/scripts-2.7
    changing mode of build/scripts-2.7/tarshards from 644 to 755
    changing mode of build/scripts-2.7/show-input from 644 to 755
    changing mode of build/scripts-2.7/training-test-split from 644 to 755
    changing mode of build/scripts-2.7/lsmodel from 644 to 755
    running install_lib
    creating /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/loader.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/zcom.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/sequence.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/utils.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/aggregator.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/gopen.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/tarrecords.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/filters.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/parallel.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/__init__.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/sqlitedb.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/sources.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/paths.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/improc.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    copying build/lib/dlinputs/loadable.py -> /opt/conda/lib/python2.7/site-packages/dlinputs
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/loader.py to loader.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/zcom.py to zcom.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/sequence.py to sequence.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/utils.py to utils.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/aggregator.py to aggregator.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/gopen.py to gopen.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/tarrecords.py to tarrecords.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/filters.py to filters.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/parallel.py to parallel.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/__init__.py to __init__.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/sqlitedb.py to sqlitedb.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/sources.py to sources.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/paths.py to paths.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/improc.py to improc.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlinputs/loadable.py to loadable.pyc
    running install_scripts
    copying build/scripts-2.7/lsmodel -> /opt/conda/bin
    copying build/scripts-2.7/training-test-split -> /opt/conda/bin
    copying build/scripts-2.7/tarshards -> /opt/conda/bin
    copying build/scripts-2.7/show-input -> /opt/conda/bin
    changing mode of /opt/conda/bin/lsmodel to 755
    changing mode of /opt/conda/bin/training-test-split to 755
    changing mode of /opt/conda/bin/tarshards to 755
    changing mode of /opt/conda/bin/show-input to 755
    running install_egg_info
    Writing /opt/conda/lib/python2.7/site-packages/dlinputs-v0.0-py2.7.egg-info
    Removing intermediate container 9035b52b4bca
     ---> e4e8f4fb301d
    Step 34/67 : COPY dltrainers /tmp/dltrainers
     ---> 5bc80d54ad87
    Step 35/67 : RUN cd /tmp/dltrainers && python setup.py install && rm -rf /tmp/dltrainers
     ---> Running in a7acdafef997
    running install
    running build
    running build_py
    creating build
    creating build/lib
    creating build/lib/dltrainers
    copying dltrainers/layers.py -> build/lib/dltrainers
    copying dltrainers/helpers.py -> build/lib/dltrainers
    copying dltrainers/__init__.py -> build/lib/dltrainers
    copying dltrainers/flex.py -> build/lib/dltrainers
    copying dltrainers/trainers.py -> build/lib/dltrainers
    running install_lib
    creating /opt/conda/lib/python2.7/site-packages/dltrainers
    copying build/lib/dltrainers/layers.py -> /opt/conda/lib/python2.7/site-packages/dltrainers
    copying build/lib/dltrainers/helpers.py -> /opt/conda/lib/python2.7/site-packages/dltrainers
    copying build/lib/dltrainers/__init__.py -> /opt/conda/lib/python2.7/site-packages/dltrainers
    copying build/lib/dltrainers/flex.py -> /opt/conda/lib/python2.7/site-packages/dltrainers
    copying build/lib/dltrainers/trainers.py -> /opt/conda/lib/python2.7/site-packages/dltrainers
    byte-compiling /opt/conda/lib/python2.7/site-packages/dltrainers/layers.py to layers.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dltrainers/helpers.py to helpers.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dltrainers/__init__.py to __init__.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dltrainers/flex.py to flex.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dltrainers/trainers.py to trainers.pyc
    running install_egg_info
    Writing /opt/conda/lib/python2.7/site-packages/dltrainers-v0.0-py2.7.egg-info
    Removing intermediate container a7acdafef997
     ---> 8a5ce1f64a70
    Step 36/67 : COPY dlmodels /tmp/dlmodels
     ---> c93176cc25f4
    Step 37/67 : RUN cd /tmp/dlmodels && python setup.py install && rm -rf /tmp/dlmodels
     ---> Running in 6039fff12175
    running install
    running build
    running build_py
    creating build
    creating build/lib
    creating build/lib/dlmodels
    copying dlmodels/layers.py -> build/lib/dlmodels
    copying dlmodels/helpers.py -> build/lib/dlmodels
    copying dlmodels/__init__.py -> build/lib/dlmodels
    copying dlmodels/specs.py -> build/lib/dlmodels
    copying dlmodels/loadable.py -> build/lib/dlmodels
    running install_lib
    creating /opt/conda/lib/python2.7/site-packages/dlmodels
    copying build/lib/dlmodels/layers.py -> /opt/conda/lib/python2.7/site-packages/dlmodels
    copying build/lib/dlmodels/helpers.py -> /opt/conda/lib/python2.7/site-packages/dlmodels
    copying build/lib/dlmodels/__init__.py -> /opt/conda/lib/python2.7/site-packages/dlmodels
    copying build/lib/dlmodels/specs.py -> /opt/conda/lib/python2.7/site-packages/dlmodels
    copying build/lib/dlmodels/loadable.py -> /opt/conda/lib/python2.7/site-packages/dlmodels
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlmodels/layers.py to layers.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlmodels/helpers.py to helpers.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlmodels/__init__.py to __init__.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlmodels/specs.py to specs.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/dlmodels/loadable.py to loadable.pyc
    running install_egg_info
    Writing /opt/conda/lib/python2.7/site-packages/dlmodels-v0.0-py2.7.egg-info
    Removing intermediate container 6039fff12175
     ---> 7f050008088b
    Step 38/67 : COPY cctc /tmp/cctc
     ---> ac3df271ef94
    Step 39/67 : RUN cd /tmp/cctc && python setup.py install && rm -rf /tmp/cctc
     ---> Running in 7c8379b66b51
    running install
    running bdist_egg
    running egg_info
    creating cctc.egg-info
    writing cctc.egg-info/PKG-INFO
    writing top-level names to cctc.egg-info/top_level.txt
    writing dependency_links to cctc.egg-info/dependency_links.txt
    writing manifest file 'cctc.egg-info/SOURCES.txt'
    reading manifest file 'cctc.egg-info/SOURCES.txt'
    writing manifest file 'cctc.egg-info/SOURCES.txt'
    installing library code to build/bdist.linux-x86_64/egg
    running install_lib
    running build_py
    creating build
    creating build/lib.linux-x86_64-2.7
    creating build/lib.linux-x86_64-2.7/cctc
    copying cctc/__init__.py -> build/lib.linux-x86_64-2.7/cctc
    running build_ext
    generating cffi module 'build/temp.linux-x86_64-2.7/_cctc.c'
    creating build/temp.linux-x86_64-2.7
    building '_cctc' extension
    creating build/temp.linux-x86_64-2.7/build
    creating build/temp.linux-x86_64-2.7/build/temp.linux-x86_64-2.7
    creating build/temp.linux-x86_64-2.7/tmp
    creating build/temp.linux-x86_64-2.7/tmp/cctc
    gcc -pthread -B /opt/conda/compiler_compat -Wl,--sysroot=/ -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -I/opt/conda/lib/python2.7/site-packages/torch/utils/ffi/../../lib/include -I/opt/conda/lib/python2.7/site-packages/torch/utils/ffi/../../lib/include/TH -I/opt/conda/include/python2.7 -c build/temp.linux-x86_64-2.7/_cctc.c -o build/temp.linux-x86_64-2.7/build/temp.linux-x86_64-2.7/_cctc.o --std=c++11 -fsanitize=leak
    [91mcc1: warning: command line option '-std=c++11' is valid for C++/ObjC++ but not for C
    [0mgcc -pthread -B /opt/conda/compiler_compat -Wl,--sysroot=/ -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -I/opt/conda/lib/python2.7/site-packages/torch/utils/ffi/../../lib/include -I/opt/conda/lib/python2.7/site-packages/torch/utils/ffi/../../lib/include/TH -I/opt/conda/include/python2.7 -c /tmp/cctc/cctc.cc -o build/temp.linux-x86_64-2.7/tmp/cctc/cctc.o --std=c++11 -fsanitize=leak
    [91mcc1plus: warning: command line option '-Wstrict-prototypes' is valid for C/ObjC but not for C++
    [0mg++ -pthread -shared -B /opt/conda/compiler_compat -L/opt/conda/lib -Wl,-rpath=/opt/conda/lib -Wl,--no-as-needed -Wl,--sysroot=/ build/temp.linux-x86_64-2.7/build/temp.linux-x86_64-2.7/_cctc.o build/temp.linux-x86_64-2.7/tmp/cctc/cctc.o -L/opt/conda/lib -lpython2.7 -o build/lib.linux-x86_64-2.7/cctc/_cctc.so
    creating build/bdist.linux-x86_64
    creating build/bdist.linux-x86_64/egg
    creating build/bdist.linux-x86_64/egg/cctc
    copying build/lib.linux-x86_64-2.7/cctc/_cctc.so -> build/bdist.linux-x86_64/egg/cctc
    copying build/lib.linux-x86_64-2.7/cctc/__init__.py -> build/bdist.linux-x86_64/egg/cctc
    byte-compiling build/bdist.linux-x86_64/egg/cctc/__init__.py to __init__.pyc
    creating stub loader for cctc/_cctc.so
    byte-compiling build/bdist.linux-x86_64/egg/cctc/_cctc.py to _cctc.pyc
    creating build/bdist.linux-x86_64/egg/EGG-INFO
    copying cctc.egg-info/PKG-INFO -> build/bdist.linux-x86_64/egg/EGG-INFO
    copying cctc.egg-info/SOURCES.txt -> build/bdist.linux-x86_64/egg/EGG-INFO
    copying cctc.egg-info/dependency_links.txt -> build/bdist.linux-x86_64/egg/EGG-INFO
    copying cctc.egg-info/top_level.txt -> build/bdist.linux-x86_64/egg/EGG-INFO
    writing build/bdist.linux-x86_64/egg/EGG-INFO/native_libs.txt
    [91mzip_safe flag not set; analyzing archive contents...
    [0mcreating dist
    creating 'dist/cctc-0.1-py2.7-linux-x86_64.egg' and adding 'build/bdist.linux-x86_64/egg' to it
    removing 'build/bdist.linux-x86_64/egg' (and everything under it)
    Processing cctc-0.1-py2.7-linux-x86_64.egg
    Copying cctc-0.1-py2.7-linux-x86_64.egg to /opt/conda/lib/python2.7/site-packages
    Adding cctc 0.1 to easy-install.pth file
    
    Installed /opt/conda/lib/python2.7/site-packages/cctc-0.1-py2.7-linux-x86_64.egg
    Processing dependencies for cctc==0.1
    Finished processing dependencies for cctc==0.1
    Removing intermediate container 7c8379b66b51
     ---> 7c141fd8e7f6
    Step 40/67 : COPY ocrobin /tmp/ocrobin
     ---> 894a7eefb867
    Step 41/67 : RUN cd /tmp/ocrobin && python setup.py install && rm -rf /tmp/ocrobin
     ---> Running in 4392eef7778d
    running install
    running build
    running build_py
    creating build
    creating build/lib
    creating build/lib/ocrobin
    copying ocrobin/__init__.py -> build/lib/ocrobin
    copying ocrobin/binarizer.py -> build/lib/ocrobin
    running build_scripts
    creating build/scripts-2.7
    copying and adjusting ocrobin-train -> build/scripts-2.7
    copying and adjusting ocrobin-pred -> build/scripts-2.7
    changing mode of build/scripts-2.7/ocrobin-train from 644 to 755
    changing mode of build/scripts-2.7/ocrobin-pred from 644 to 755
    running install_lib
    creating /opt/conda/lib/python2.7/site-packages/ocrobin
    copying build/lib/ocrobin/__init__.py -> /opt/conda/lib/python2.7/site-packages/ocrobin
    copying build/lib/ocrobin/binarizer.py -> /opt/conda/lib/python2.7/site-packages/ocrobin
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocrobin/__init__.py to __init__.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocrobin/binarizer.py to binarizer.pyc
    running install_scripts
    copying build/scripts-2.7/ocrobin-train -> /opt/conda/bin
    copying build/scripts-2.7/ocrobin-pred -> /opt/conda/bin
    changing mode of /opt/conda/bin/ocrobin-train to 755
    changing mode of /opt/conda/bin/ocrobin-pred to 755
    running install_data
    creating /opt/conda/share/ocrobin
    copying bin-000000046-005393.pt -> /opt/conda/share/ocrobin
    running install_egg_info
    Writing /opt/conda/lib/python2.7/site-packages/ocrobin-v0.0-py2.7.egg-info
    Removing intermediate container 4392eef7778d
     ---> d0c9c8312f63
    Step 42/67 : COPY ocrodeg /tmp/ocrodeg
     ---> 212d9c32c4d0
    Step 43/67 : RUN cd /tmp/ocrodeg && python setup.py install && rm -rf /tmp/ocrodeg
     ---> Running in f83a59dd8a2f
    running install
    running build
    running build_py
    creating build
    creating build/lib
    creating build/lib/ocrodeg
    copying ocrodeg/degrade.py -> build/lib/ocrodeg
    copying ocrodeg/__init__.py -> build/lib/ocrodeg
    running install_lib
    creating /opt/conda/lib/python2.7/site-packages/ocrodeg
    copying build/lib/ocrodeg/degrade.py -> /opt/conda/lib/python2.7/site-packages/ocrodeg
    copying build/lib/ocrodeg/__init__.py -> /opt/conda/lib/python2.7/site-packages/ocrodeg
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocrodeg/degrade.py to degrade.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocrodeg/__init__.py to __init__.pyc
    running install_egg_info
    Writing /opt/conda/lib/python2.7/site-packages/ocrodeg-v0.0-py2.7.egg-info
    Removing intermediate container f83a59dd8a2f
     ---> dd978ea9ab30
    Step 44/67 : COPY ocroline /tmp/ocroline
     ---> acc91134a7ed
    Step 45/67 : RUN cd /tmp/ocroline && python setup.py install && rm -rf /tmp/ocroline
     ---> Running in bcf6f4f5f00d
    downloading https://storage.googleapis.com/tmb-models/line2-000003330-004377.pt
    running install
    running build
    running build_py
    creating build
    creating build/lib
    creating build/lib/ocroline
    copying ocroline/__init__.py -> build/lib/ocroline
    copying ocroline/recognizer.py -> build/lib/ocroline
    copying ocroline/lineest.py -> build/lib/ocroline
    running build_scripts
    creating build/scripts-2.7
    copying and adjusting ocroline-train -> build/scripts-2.7
    copying and adjusting ocroline-pred -> build/scripts-2.7
    changing mode of build/scripts-2.7/ocroline-train from 644 to 755
    changing mode of build/scripts-2.7/ocroline-pred from 644 to 755
    running install_lib
    creating /opt/conda/lib/python2.7/site-packages/ocroline
    copying build/lib/ocroline/__init__.py -> /opt/conda/lib/python2.7/site-packages/ocroline
    copying build/lib/ocroline/recognizer.py -> /opt/conda/lib/python2.7/site-packages/ocroline
    copying build/lib/ocroline/lineest.py -> /opt/conda/lib/python2.7/site-packages/ocroline
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocroline/__init__.py to __init__.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocroline/recognizer.py to recognizer.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocroline/lineest.py to lineest.pyc
    running install_scripts
    copying build/scripts-2.7/ocroline-pred -> /opt/conda/bin
    copying build/scripts-2.7/ocroline-train -> /opt/conda/bin
    changing mode of /opt/conda/bin/ocroline-pred to 755
    changing mode of /opt/conda/bin/ocroline-train to 755
    running install_data
    creating /opt/conda/share/ocroline
    copying line2-000003330-004377.pt -> /opt/conda/share/ocroline
    running install_egg_info
    Writing /opt/conda/lib/python2.7/site-packages/ocroline-v0.0-py2.7.egg-info
    Removing intermediate container bcf6f4f5f00d
     ---> 2316bee20add
    Step 46/67 : COPY ocrorot /tmp/ocrorot
     ---> 00c917988291
    Step 47/67 : RUN cd /tmp/ocrorot && python setup.py install && rm -rf /tmp/ocrorot
     ---> Running in 850f64ee4cf4
    downloading https://storage.googleapis.com/tmb-models/logskew-000015808-000132.pt
    running install
    running build
    running build_py
    creating build
    creating build/lib
    creating build/lib/ocrorot
    copying ocrorot/rotation.py -> build/lib/ocrorot
    copying ocrorot/__init__.py -> build/lib/ocrorot
    running build_scripts
    creating build/scripts-2.7
    copying and adjusting ocrorot-train -> build/scripts-2.7
    copying and adjusting ocroskew-train -> build/scripts-2.7
    copying and adjusting ocrorot-pred -> build/scripts-2.7
    copying and adjusting ocroskew-pred -> build/scripts-2.7
    changing mode of build/scripts-2.7/ocrorot-train from 644 to 755
    changing mode of build/scripts-2.7/ocroskew-train from 644 to 755
    changing mode of build/scripts-2.7/ocrorot-pred from 644 to 755
    changing mode of build/scripts-2.7/ocroskew-pred from 644 to 755
    running install_lib
    creating /opt/conda/lib/python2.7/site-packages/ocrorot
    copying build/lib/ocrorot/rotation.py -> /opt/conda/lib/python2.7/site-packages/ocrorot
    copying build/lib/ocrorot/__init__.py -> /opt/conda/lib/python2.7/site-packages/ocrorot
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocrorot/rotation.py to rotation.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocrorot/__init__.py to __init__.pyc
    running install_scripts
    copying build/scripts-2.7/ocroskew-train -> /opt/conda/bin
    copying build/scripts-2.7/ocroskew-pred -> /opt/conda/bin
    copying build/scripts-2.7/ocrorot-train -> /opt/conda/bin
    copying build/scripts-2.7/ocrorot-pred -> /opt/conda/bin
    changing mode of /opt/conda/bin/ocroskew-train to 755
    changing mode of /opt/conda/bin/ocroskew-pred to 755
    changing mode of /opt/conda/bin/ocrorot-train to 755
    changing mode of /opt/conda/bin/ocrorot-pred to 755
    running install_data
    creating /opt/conda/share/ocrorot
    copying rot-000003456-020897.pt -> /opt/conda/share/ocrorot
    copying logskew-000015808-000132.pt -> /opt/conda/share/ocrorot
    running install_egg_info
    Writing /opt/conda/lib/python2.7/site-packages/ocrorot-v0.0-py2.7.egg-info
    Removing intermediate container 850f64ee4cf4
     ---> 79378f57c9f6
    Step 48/67 : COPY ocroseg /tmp/ocroseg
     ---> 5f6644aa59dc
    Step 49/67 : RUN cd /tmp/ocroseg && python setup.py install && rm -rf /tmp/ocroseg
     ---> Running in a04a9314b3b3
    running install
    running build
    running build_py
    creating build
    creating build/lib
    creating build/lib/ocroseg
    copying ocroseg/degrade.py -> build/lib/ocroseg
    copying ocroseg/__init__.py -> build/lib/ocroseg
    copying ocroseg/psegutils.py -> build/lib/ocroseg
    copying ocroseg/segmentation.py -> build/lib/ocroseg
    running build_scripts
    creating build/scripts-2.7
    copying and adjusting ocroseg-train -> build/scripts-2.7
    copying and adjusting ocroseg-pred -> build/scripts-2.7
    copying and adjusting ocroseg-predlines -> build/scripts-2.7
    changing mode of build/scripts-2.7/ocroseg-train from 644 to 755
    changing mode of build/scripts-2.7/ocroseg-pred from 644 to 755
    changing mode of build/scripts-2.7/ocroseg-predlines from 644 to 755
    running install_lib
    creating /opt/conda/lib/python2.7/site-packages/ocroseg
    copying build/lib/ocroseg/degrade.py -> /opt/conda/lib/python2.7/site-packages/ocroseg
    copying build/lib/ocroseg/__init__.py -> /opt/conda/lib/python2.7/site-packages/ocroseg
    copying build/lib/ocroseg/psegutils.py -> /opt/conda/lib/python2.7/site-packages/ocroseg
    copying build/lib/ocroseg/segmentation.py -> /opt/conda/lib/python2.7/site-packages/ocroseg
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocroseg/degrade.py to degrade.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocroseg/__init__.py to __init__.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocroseg/psegutils.py to psegutils.pyc
    byte-compiling /opt/conda/lib/python2.7/site-packages/ocroseg/segmentation.py to segmentation.pyc
    running install_scripts
    copying build/scripts-2.7/ocroseg-pred -> /opt/conda/bin
    copying build/scripts-2.7/ocroseg-train -> /opt/conda/bin
    copying build/scripts-2.7/ocroseg-predlines -> /opt/conda/bin
    changing mode of /opt/conda/bin/ocroseg-pred to 755
    changing mode of /opt/conda/bin/ocroseg-train to 755
    changing mode of /opt/conda/bin/ocroseg-predlines to 755
    running install_data
    creating /opt/conda/share/ocroseg
    copying lowskew-000000259-011440.pt -> /opt/conda/share/ocroseg
    running install_egg_info
    Writing /opt/conda/lib/python2.7/site-packages/ocroseg-v0.0-py2.7.egg-info
    Removing intermediate container a04a9314b3b3
     ---> b83f36475e25
    Step 50/67 : ENV USER user
     ---> Running in d92f26827cd5
    Removing intermediate container d92f26827cd5
     ---> 329534fd4b73
    Step 51/67 : ENV HOME /home/$USER
     ---> Running in 20280401a8a7
    Removing intermediate container 20280401a8a7
     ---> c66b2633744a
    Step 52/67 : ENV GID 1000
     ---> Running in 4d170c102200
    Removing intermediate container 4d170c102200
     ---> 1d1a4e08fadc
    Step 53/67 : ENV UID 1000
     ---> Running in 78a84afea7d7
    Removing intermediate container 78a84afea7d7
     ---> f500c1c5c282
    Step 54/67 : ENV TERM xterm
     ---> Running in 08fd4c6e2da1
    Removing intermediate container 08fd4c6e2da1
     ---> dfdfde9b25e0
    Step 55/67 : ENV LD_LIBRARY_PATH /usr/local/nvidia/lib:/usr/local/nvidia/lib64
     ---> Running in b999151d829f
    Removing intermediate container b999151d829f
     ---> 3ea5ec44c36d
    Step 56/67 : ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
     ---> Running in fb9768ccb380
    Removing intermediate container fb9768ccb380
     ---> 6d5c5632ee2b
    Step 57/67 : COPY nginx.conf /etc/nginx
     ---> 2bd28ae908bb
    Step 58/67 : EXPOSE 3218
     ---> Running in d279ec43847f
    Removing intermediate container d279ec43847f
     ---> c5c7e835c19d
    Step 59/67 : RUN mkdir -p $HOME && groupadd -g $GID -r $USER && useradd --no-log-init -u $UID -r -g $USER $USER
     ---> Running in d2d3916f6b3b
    Removing intermediate container d2d3916f6b3b
     ---> 5ee20ccdaab9
    Step 60/67 : RUN mkdir -p $HOME/.jupyter $HOME/.vnc $HOME/.ssh
     ---> Running in 563522b6e55b
    Removing intermediate container 563522b6e55b
     ---> 0a912ac7a04a
    Step 61/67 : ADD jupyter_config/* $HOME/.jupyter/
     ---> 8d4200a165cc
    Step 62/67 : ADD vnc_config/* $HOME/.vnc/
     ---> 6f1fd55c6a7e
    Step 63/67 : ADD scripts/* /usr/local/bin/
     ---> db0c8137466e
    Step 64/67 : RUN true     && echo ". /opt/conda/etc/profile.d/conda.sh" >> $HOME/.bashrc     && echo "conda activate base" >> $HOME/.bashrc     && chown -R $USER.$USER $HOME
     ---> Running in e60b55285640
    Removing intermediate container e60b55285640
     ---> 445f54a741fc
    Step 65/67 : RUN echo 'user ALL=(ALL:ALL) NOPASSWD:ALL' >> /etc/sudoers
     ---> Running in 39d32189d90d
    Removing intermediate container 39d32189d90d
     ---> e4114a076054
    Step 66/67 : USER $UID
     ---> Running in 017bedc870ff
    Removing intermediate container 017bedc870ff
     ---> 10c9fd428517
    Step 67/67 : ENTRYPOINT ["runcmd"]
     ---> Running in 191729e0af87
    Removing intermediate container 191729e0af87
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
!./ocropy ocroseg-train -d http://storage.googleapis.com/lpr-ocr/uw3-framed-lines.tgz
```

    + ocroseg-train -d http://storage.googleapis.com/lpr-ocr/uw3-framed-lines.tgz
    raw sample:
    __key__ 'A001BIN'
    __source__ 'http://storage.googleapis.com/lpr-ocr/uw3-framed-lines.tgz'
    lines.png float32 (3300, 2592)
    png float32 (3300, 2592)
    
    preprocessed sample:
    __key__ <type 'list'> ['A005BIN']
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
    
    0 0 ['A006BIN'] 0.2556696 ['A006BIN'] 0.4947549 0.52179205 lr 0.03
    QStandardPaths: XDG_RUNTIME_DIR not set, defaulting to '/tmp/runtime-user'
    Qt: XKEYBOARD extension not present on the X server.
    1 1 ['A002BIN'] 0.25626054 ['A002BIN'] 0.49809185 0.5318549 lr 0.03
    2 2 ['A008BIN'] 0.25159708 ['A008BIN'] 0.4915368 0.5178368 lr 0.03
    3 3 ['A007BIN'] 0.24971302 ['A007BIN'] 0.48830062 0.5145237 lr 0.03
    4 4 ['A00EBIN'] 0.24829507 ['A00EBIN'] 0.49240503 0.5169772 lr 0.03
    5 5 ['A00IBIN'] 0.24613555 ['A00IBIN'] 0.4887335 0.5155512 lr 0.03
    6 6 ['A00MBIN'] 0.24369389 ['A00MBIN'] 0.47928345 0.51025665 lr 0.03
    ^C
    Traceback (most recent call last):
      File "/opt/conda/bin/ocroseg-train", line 262, in <module>
        for sample in source:
      File "/opt/conda/lib/python2.7/site-packages/dlinputs/filters.py", line 395, in batched
        for sample in data:
      File "/opt/conda/lib/python2.7/site-packages/dlinputs/filters.py", line 228, in ren
        for sample in data:
      File "/opt/conda/lib/python2.7/site-packages/dlinputs/filters.py", line 325, in transform
        for sample in data:
      File "/opt/conda/lib/python2.7/site-packages/dlinputs/filters.py", line 350, in shuffle
        for sample in data:
      File "/opt/conda/lib/python2.7/site-packages/dlinputs/filters.py", line 228, in ren
        for sample in data:
      File "/opt/conda/lib/python2.7/site-packages/dlinputs/gopen.py", line 79, in sharditerator
        for sample in tarrecords.tariterator(stream, **kw):
      File "/opt/conda/lib/python2.7/site-packages/dlinputs/tarrecords.py", line 164, in tariterator
        yield decode(current_sample)
      File "/opt/conda/lib/python2.7/site-packages/dlinputs/utils.py", line 258, in autodecode
        result[k] = autodecode1(v, k)
      File "/opt/conda/lib/python2.7/site-packages/dlinputs/utils.py", line 222, in autodecode1
        result = imageio.imread(stream)
      File "/opt/conda/lib/python2.7/site-packages/imageio/core/functions.py", line 206, in imread
        reader = read(uri, format, 'i', **kwargs)
      File "/opt/conda/lib/python2.7/site-packages/imageio/core/functions.py", line 129, in get_reader
        return format.get_reader(request)
      File "/opt/conda/lib/python2.7/site-packages/imageio/core/format.py", line 168, in get_reader
        return self.Reader(self, request)
      File "/opt/conda/lib/python2.7/site-packages/imageio/core/format.py", line 217, in __init__
        self._open(**self.request.kwargs.copy())
      File "/opt/conda/lib/python2.7/site-packages/imageio/plugins/pillow.py", line 277, in _open
        pilmode=pilmode, as_gray=as_gray)
      File "/opt/conda/lib/python2.7/site-packages/imageio/plugins/pillow.py", line 125, in _open
        pil_try_read(self._im)
      File "/opt/conda/lib/python2.7/site-packages/imageio/plugins/pillow.py", line 491, in pil_try_read
        im.getdata()[0]
      File "/opt/conda/lib/python2.7/site-packages/PIL/Image.py", line 1220, in getdata
        self.load()
      File "/opt/conda/lib/python2.7/site-packages/PIL/ImageFile.py", line 231, in load
        n, err_code = decoder.decode(b)
    KeyboardInterrupt


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

