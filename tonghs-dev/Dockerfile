FROM debian:7.8 

MAINTAINER tonghs <tonghuashuai@gmail.com>

# 网易源
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak
COPY ./bjt.list /etc/apt/sources.list

# 更新源 && 安装必要软件
RUN apt-get update && apt-get install -y python mlocate wget vim tmux silversearcher-ag git mercurial  nginx  redis-server openssh-server python-pip sudo
RUN pip install tornado mako tmuxp redis

# 更新 locate 缓存
RUN updatedb

# 创建用户
RUN mkdir /home/tonghs
RUN useradd -s /bin/bash -G root -d /home/tonghs tonghs
RUN chown -R tonghs /home/tonghs
# 添加 sudo
RUN echo 'tonghs ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# 配置 sshd
RUN mkdir -p /var/run/sshd

# 配置公钥
RUN sudo -u tonghs mkdir -p /home/tonghs/.ssh
ADD authorized_keys /home/tonghs/.ssh/authorized_keys

# 配置文件
RUN sudo -u tonghs mkdir -p /home/tonghs/git 

WORKDIR /home/tonghs/git
RUN git clone https://github.com/tonghuashuai/myfile.git && chown -R tonghs myfile

WORKDIR /home/tonghs/git/myfile
RUN sudo -u tonghs cp vim /home/tonghs/.vim -rf && sudo -u tonghs cp vimrc /home/tonghs/.vimrc && sudo -u tonghs cp bash /home/tonghs/.bash_tonghs

WORKDIR /home/tonghs
RUN sudo -u tonghs echo 'if [ -f ~/.bash_tonghs]; then' >> /home/tonghs/.bashrc
RUN sudo -u tonghs echo '  . ~/.bash_tonghs' >> /home/tonghs/.bashrc
RUN sudo -u tonghs echo 'fi' >> /home/tonghs/.bashrc

RUN sudo -u tonghs echo 'if [ -f ~/.bash_tonghs]; then' >> /home/tonghs/.bash_profile
RUN sudo -u tonghs echo '  . ~/.bash_tonghs' >> /home/tonghs/.bash_profile
RUN sudo -u tonghs echo 'fi' >> /home/tonghs/.bash_profile

RUN sudo -u tonghs git clone https://github.com/gmarik/Vundle.vim.git /home/tonghs/.vim/bundle/Vundle.vim

ADD run.sh /run.sh
RUN chmod 755 /run.sh

# 开放端口
EXPOSE 22 8080 80

CMD ["/run.sh"]
