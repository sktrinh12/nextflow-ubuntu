FROM ubuntu:20.04

ARG TOWER_ACCESS_TOKEN
ARG PATH="/root/miniconda3/bin:$PATH"
ENV TOWER_ACCESS_TOKEN=$TOWER_ACCESS_TOKEN
ENV JAVA_HOME=/usr/local/java
ENV DEBIAN_FRONTEND=noninteractive
ENV PATH="/root/miniconda3/bin:$PATH"

RUN apt-get update && \
  apt-get install -y default-jre wget curl git neovim graphviz

RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
-O miniconda.sh && bash miniconda.sh -b -u -p /root/miniconda3 &&\
. /root/.bashrc && rm -f miniconda.sh

# -b, run install in batch mode (without manual intervention)
# -u, update an existing installation
# -p, install prefix, defaults to /root/miniconda3, must not contain spaces

RUN git clone https://github.com/sktrinh12/nvim-lua-config.git &&\
  mkdir -p /root/.config/nvim/ &&\
  mv nvim-lua-config/* /root/.config/nvim/ &&\
  nvim +'PlugInstall --sync' +qa

RUN curl get.nextflow.io | bash

RUN mv nextflow /usr/local/bin

# COPY environment.yml .
# RUN conda env create -f environment.yml
RUN conda create -c bioconda -n genetics salmon fastqc multiqc

# SHELL ["conda", "run", "--no-capture-output", "-n", "genetics", "/bin/bash", "-c"]

ENTRYPOINT ["conda", "run", "--no-capture-output", "-n", "genetics", "/bin/bash", "-c"]
# --memory="1g"
# docker run -it --memory="1g" --memory-swap="2g" ubuntu
