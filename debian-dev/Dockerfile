FROM ubuntu:14.10

MAINTAINER tonghs <tonghuashuai@gmail.com>

# 网易源
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak
COPY ./bjt.list /etc/apt/sources.list

# 更新源 && 安装必要软件
RUN apt-get update && apt-get install -y python mlocate wget vim tmux silversearcher-ag git mercurial  nginx  redis-server openssh-server

# 更新 locate 缓存
RUN updatedb

RUN wget https://bootstrap.pypa.io/get-pip.py

RUN python get-pip.py

RUN rm get-pip.py

RUN pip install tornado mako tmuxp redis

# 配置 sshd
RUN mkdir -p /var/run/sshd

# 配置公钥
RUN mkdir -p /root/.ssh
ADD authorized_keys /root/.ssh/authorized_keys

ADD run.sh /run.sh
RUN chmod 755 /run.sh

# 开放端口
EXPOSE 22

CMD ["/run.sh"]
