FROM alpine:latest 

LABEL Maintainer="CatDEV <dev.catnet@gmail.com>"

### ENV 
ENV TZ=America/Guatemala

### COPY
COPY ./PRC/motd /etc/motd
COPY ./PRC/profile /etc/profile
COPY ./PRC/authorized_keys /root/.ssh/authorized_keys
COPY SCP /SCP

### [ RUN ]
### [!]: Replace \ + && with ;\
RUN set -eux ;\
# [NOTA]: Los siguientes repositorios estaban activados por defecto en versiones anteriores, pero ahora se reservan solo para la version Alpine Legacy y Alpine Test.
#         Se activaran solamente si es nesesario y en la imagen que lo sea, por defecto permaneceran inactivos.
    #echo "http://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories ;\
    #echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories ;\
    #echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories ;\
### Install Basic Packages
    apk --no-cache add \
### Bash
        bash \
### SSH
        openssh \
### Basic Packages
# [NOTA]: ncurses fue desactivado por no ser binario nesesario.
        wget \
        curl \
        nano \
        tzdata \        
        # ncurses \
        supervisor \
        sudo ;\
### Set-up SHH Server
    ssh-keygen -A ;\
    sed -i s/#PermitRootLogin.*/PermitRootLogin\ yes/ /etc/ssh/sshd_config ;\
    echo "root:Shaka.66" | chpasswd ;\
### Setting bash as default in root
    sed -i s/ash/bash/g /etc/passwd ;\
### Setting the Timezone
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone ;\
### Add Sudoers User
    addgroup alpine ;\
    adduser  -G alpine -s /bin/sh -D alpine ;\
    echo "alpine:alpine" | /usr/sbin/chpasswd ;\
    echo "alpine    ALL=(ALL) ALL" >> /etc/sudoers ;\
### Set authorized_keys 
    chmod 700 /root/.ssh ;\
    chmod 600 /root/.ssh/authorized_keys ;\
### Set Script Dir
    chmod +x /SCP/* ;\
### Set COM Dir
    mkdir /COM ;\
    chmod -R 777 /COM ;\
### Clean [!]
    /SCP/clean.sh

ADD ./CFG/supervisord.conf /etc/supervisord.conf

### [ EXPOSE ]
EXPOSE 22

### [ CMD ]
CMD ["/usr/bin/supervisord","-c","/etc/supervisord.conf"]