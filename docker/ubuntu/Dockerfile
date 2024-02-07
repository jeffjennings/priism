FROM ubuntu:20.04

# install required packages
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
    apt-get install -y \
    sudo \
    wget \
    cmake \
    g++ \
    libfftw3-bin \
    libfftw3-dev \
    make \
    python3 \
    python3-dev \
    python3-pip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# create user for running PRIISM
RUN echo "root:root" | chpasswd

# run the following commands as anonymous
CMD /bin/bash

# working directory is home directory
ENV HOME /home/
WORKDIR ${HOME}

# upgrade pip
RUN python3 -m pip install --upgrade --no-cache-dir pip

# download priism
RUN python3 -c "import io, urllib.request as request, zipfile ; req = request.urlopen('https://github.com/jeffjennings/priism/archive/refs/heads/docker_tweaks.zip') ; f=io.BytesIO(req.read()) ; zf = zipfile.ZipFile(f, mode='r') ; zf.extractall() ; zf.close()" \
    && mv priism-docker_tweaks priism

# change directory
WORKDIR ${HOME}/priism

# install python dependencies for PRIISM
RUN python3 -m pip install --no-cache-dir --no-warn-script-location -r requirements.txt

# build & install PRIISM
RUN python3 setup.py build && python3 setup.py install

# install Jupyter and astropy
RUN python3 -m pip install --no-cache-dir --no-warn-script-location jupyter astropy
ENV PATH ${HOME}/.local/bin:${PATH}

# go back to home directory
WORKDIR ${HOME}
