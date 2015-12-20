FROM ubuntu:14.04 

MAINTAINER tonghs <tonghuashuai@gmail.com>

# 更新源 && 安装 ssh
COPY ./sources.list /etc/apt/sources.list
RUN apt-get update && apt-get install -y openssh-server

# 配置 sshd
RUN mkdir -p /var/run/sshd

# 配置公钥
RUN mkdir -p /root/.ssh
ADD authorized_keys /root/.ssh/authorized_keys


RUN echo "root:root" | chpasswd

# 开放端口
EXPOSE 22 80 8080

ADD run.sh /run.sh
RUN chmod 755 /run.sh
CMD ["/run.sh"]
