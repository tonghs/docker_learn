FROM ac-dev:ubuntu 

MAINTAINER tonghs<tonghuashuai@gmail.com>

# 更新源 && 安装 tool
RUN apt-get update && apt-get install -y mlocate wget vim tmux silversearcher-ag git mercurial sudo 
#
# 更新 locate 缓存
RUN updatedb

# 创建用户
RUN mkdir /home/web
RUN useradd -s /bin/bash -G root -d /home/web web 
RUN chown -R web /home/web
RUN echo 'web:pevcpevc' | chpasswd

# 添加公钥
RUN sudo -u web mkdir -p /home/web/.ssh
ADD authorized_keys /home/web/.ssh/authorized_keys
RUN chown web:web /home/web/.ssh/authorized_keys


# 添加 sudo
RUN echo 'web ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# 开放端口
EXPOSE 22 80 8080

ADD run.sh /run.sh
RUN chmod 755 /run.sh

CMD ["/run.sh"]
