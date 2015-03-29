FROM ubuntu:14.04

VOLUME /dev /dev

# Install dependencies.
RUN apt-get update && apt-get install -y dkms git curl build-essential

# Install linux headers - use 3.18.6 version for CoreOS 607.0.0
RUN git clone -b v3.18-rc6 --depth 1 https://github.com/torvalds/linux.git /usr/src/linux \
    && mkdir -p /lib/modules/3.18.6/misc /usr/share/mime/packages \
    && ln -s /usr/src/linux /lib/modules/3.18.6/build \
    && ln -s /usr/src/linux /lib/modules/3.18.6/include \
    && touch /lib/modules/3.18.6/modules.builtin \
    && touch /lib/modules/3.18.6/modules.order \
    && cd /usr/src/linux && make mrproper && zcat /proc/config.gz > .config \
    && make oldconfig && make modules_prepare \
    && echo '#define UTS_RELEASE "3.18.6"' > /usr/src/linux/include/generated/utsrelease.h

# Install VirtualBox.
RUN curl -sSOJL http://download.virtualbox.org/virtualbox/4.3.26/VirtualBox-4.3.26-98988-Linux_amd64.run \
    && curl -sSOJL http://download.virtualbox.org/virtualbox/4.3.26/Oracle_VM_VirtualBox_Extension_Pack-4.3.26-98988.vbox-extpack \
    && sh VirtualBox-4.3.26-98988-Linux_amd64.run \
    && VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-4.3.26-98988.vbox-extpack \
    && rm -f VirtualBox-*.run Oracle_*.vbox-extpack
