FROM ubuntu:16.04

MAINTAINER tonghs <tonghuashuai@gmail.com>

# 新建用户
RUN groupadd tonghs
RUN useradd -d /home/tonghs -g tonghs -G root -m -s /bin/bash -p tonghs tonghs

# 修改中国源
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak    

RUN echo "deb http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted universe multiverse\n\
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-security main restricted universe multiverse\n\
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-updates main restricted universe multiverse\n\
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse\n\
##测试版源\n\
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-proposed main restricted universe multiverse\n\
# 源码\n\
deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted universe multiverse\n\
deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-security main restricted universe multiverse\n\
deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-updates main restricted universe multiverse\n\
deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse\n\
##测试版源\n\
deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-proposed main restricted universe multiverse" > /etc/apt/sources.list

# 更新源 && 安装必要软件
RUN apt-get update && apt-get install -y python wget vim tmux silversearcher-ag git nginx openssh-server python-pip htop python-dev unzip xtail supervisor sudo

# 安装 redis-server
RUN apt-get install -y redis-server

# sudo 免密码
RUN echo "tonghs ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# 安装 mysql 并设置密码
ENV DEBIAN_FRONTEND="noninteractive"
# RUN ["/bin/sh", "-c", "debconf-set-selections", "<<<", "mysql-server mysql-server/root-pass password tonghs"]
# RUN ["/bin/sh", "-c", "debconf-set-selections", "<<<", "mysql-server mysql-server/re-root-pass password tonghs"]
RUN apt-get install -y mysql-server
# RUN mysql -u root -e "set password for 'root'@'localhost' = password('tonghs');"

# 配置公钥及文件权限
RUN mkdir -p /home/tonghs/.ssh
RUN mkdir -p /root/.ssh

RUN echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDFZpCoIqzSDyk+XjGn4SkTpIfnLGtZO4sVeYsZlaai1tMMewaCvBm3oaDOA8zLxU745GcoPcJjYpMTt7vtOqxUWr61IU2aRCB5IIZmMBrm7ON6pmlY2D/waT2/cV450xNWcRO6tJSy6640fN6ERygf1rRMTuadYCqfIZ4ceQBCySpWX7KUhpIP0ZbeyIoEuUxiLg/7vQJWJj5z/EYBlCZh3RyE0s3fSJxgXNUqSvUs5yKsEd1ioQJOpJqKildvEO95fvjMv3DZgqd2TS+jvIOfIcNQ2s78ZsJl8Wu54/8bN1ZiEzJDfqSqkydWUOqmOJ3VmA+DT2/c6DAp6Fe+oLC7 tonghs" > /home/tonghs/.ssh/authorized_keys
RUN echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDFZpCoIqzSDyk+XjGn4SkTpIfnLGtZO4sVeYsZlaai1tMMewaCvBm3oaDOA8zLxU745GcoPcJjYpMTt7vtOqxUWr61IU2aRCB5IIZmMBrm7ON6pmlY2D/waT2/cV450xNWcRO6tJSy6640fN6ERygf1rRMTuadYCqfIZ4ceQBCySpWX7KUhpIP0ZbeyIoEuUxiLg/7vQJWJj5z/EYBlCZh3RyE0s3fSJxgXNUqSvUs5yKsEd1ioQJOpJqKildvEO95fvjMv3DZgqd2TS+jvIOfIcNQ2s78ZsJl8Wu54/8bN1ZiEzJDfqSqkydWUOqmOJ3VmA+DT2/c6DAp6Fe+oLC7 tonghs" > /root/.ssh/authorized_keys

RUN chmod 600 /root/.ssh/authorized_keys
RUN chmod 600 /home/tonghs/.ssh/authorized_keys

RUN chown tonghs:tonghs /home/tonghs/.ssh/authorized_keys


# ssh 启动所需
RUN mkdir /var/run/sshd

# ssh 自动启动
RUN echo "#!/bin/bash\n\
/usr/sbin/sshd -D" > /run.sh
RUN chmod 755 /run.sh

# 配置 bash 及 vim 环境
RUN echo "alias ls='ls --color'\n\
alias ll='ls -lh --color'\n\
alias l='ls -l --color'\n\
alias cc=\"find . -name '*.pyc' -delete\"\n\
alias c='clear'\n\
\n\
# sudo locale-gen zh_CN.UTF-8\n\
\n\
PS1='\[\e[1;32m\]\u@\h \[\e[1;34m\]\w$ \[\e[m\]'" > /home/tonghs/.bash_profile

RUN wget -O /home/tonghs/.vimrc https://raw.githubusercontent.com/tonghuashuai/myfile/master/vimrc
RUN git clone https://github.com/VundleVim/Vundle.vim.git /home/tonghs/.vim/bundle/Vundle.vim

# 开放端口
EXPOSE 22 80 8080 3000 9000 3306 6379

CMD ["/run.sh"]
