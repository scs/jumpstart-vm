FROM ubuntu:22.04

# make dpkg noninteractive
ENV DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    software-properties-common \
    git \
    python3-pip \
  && rm -rf /var/lib/apt/lists/*

RUN pip3 install \
  ansible==8.5.0 \
  ansible-lint==6.21.1 \
  yamllint==1.32.0

WORKDIR /ansible
