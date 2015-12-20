FROM ac-dev:ubuntu-tool

MAINTAINER tonghs<tonghuashuai@gmail.com>

# 更新源 && 安装 tool
RUN apt-get update && apt-get install -y python python-pip coffeescript gearman-job-server
RUN pip install ipython mako tornado pingpp qiniu mongokit redis gearman intstr pypinyin jieba sqlalchemy
