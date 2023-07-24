# https://github.com/chrisbensch/container-xrdp

FROM ubuntu:latest

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get -y update && \
    apt-get -y upgrade && \
    apt-get install -y \
    software-properties-common \
    curl \
    net-tools \
    openjdk-17-jre \
    xfce4 \
    xfce4-clipman-plugin \
    xfce4-cpugraph-plugin \
    xfce4-netload-plugin \
    xfce4-screenshooter \
    xfce4-taskmanager \
    xfce4-terminal \
    xfce4-xkb-plugin \
    sudo \
    wget \
    xorgxrdp \
    xrdp && \
    apt remove -y light-locker xscreensaver && \
    apt autoremove -y && \
    rm -rf /var/cache/apt /var/lib/apt/lists

COPY ./build/ubuntu-run.sh /usr/bin/
RUN mv /usr/bin/ubuntu-run.sh /usr/bin/run.sh
RUN chmod +x /usr/bin/run.sh

# Add .deb based Firefox instead of snap version
COPY ./build/mozilla-firefox /etc/apt/preferences.d/
RUN add-apt-repository ppa:mozillateam/ppa && \
    #apt-get -y update && \
    apt-get -y install firefox && \
    apt autoremove -y && \
    rm -rf /var/cache/apt /var/lib/apt/lists


# https://github.com/danielguerra69/ubuntu-xrdp/blob/master/Dockerfile
RUN mkdir /var/run/dbus && \
    cp /etc/X11/xrdp/xorg.conf /etc/X11 && \
    sed -i "s/console/anybody/g" /etc/X11/Xwrapper.config && \
    sed -i "s/xrdp\/xorg/xorg/g" /etc/xrdp/sesman.ini && \
    echo "xfce4-session" >> /etc/skel/.Xsession


# Docker config
EXPOSE 3389
ENTRYPOINT ["/usr/bin/run.sh"]

