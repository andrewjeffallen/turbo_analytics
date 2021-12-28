FROM ubuntu:latest
FROM continuumio/miniconda3

COPY . .

# ==================MSSQL DRIVER========================
# build variables.
ENV DEBIAN_FRONTEND noninteractive

# install Microsoft SQL Server requirements.
ENV ACCEPT_EULA=Y
RUN apt-get update -y && apt-get update \
      && apt-get install -y --no-install-recommends curl gcc g++ gnupg unixodbc unixodbc-dev
# Add SQL Server ODBC Driver 17 for Ubuntu 18.04
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
      && curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list \
      && apt-get update \
      && apt-get install -y --no-install-recommends --allow-unauthenticated msodbcsql17 mssql-tools \
      && echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile \
      && echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc


# ==================PYTHON========================

RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get dist-upgrade -y
COPY requirements.txt requirements.txt

RUN conda update -n base -c defaults conda
RUN conda create -n env python=3.7
RUN echo 'conda init bash' >/.bashrc
RUN echo "source activate env" > ~/.bashrc

ENV PATH /opt/conda/envs/env/bin:$PATH
RUN apt-get update && apt-get install -y \
      apt-utils apt-transport-https debconf-utils build-essential apt-transport-https \
      && rm -rf /var/lib/apt/lists/*
RUN apt-get update && apt-get install -y \
      python3-pip python-dev python-setuptools \
      --no-install-recommends \
      && rm -rf /var/lib/apt/lists/*

RUN pip3 install --upgrade pip

RUN conda install --name env -c conda-forge --file requirements.txt
SHELL ["conda", "run", "-n", "env", "/bin/bash", "-c"]
