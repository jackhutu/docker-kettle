FROM ubuntu:16.04

MAINTAINER Jack Hu <hello@jackhu.top>

# 安装jdk
RUN apt-get update && \
    apt-get install -y openjdk-8-jdk && \
    apt-get autoclean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/*

# 设置环境变量
ENV PDI_RELEASE=6.1 \
    PDI_VERSION=6.1.0.1-196 \
    CARTE_PORT=8181 \
    PENTAHO_JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 \
    PENTAHO_HOME=/opt/pentaho

# 创建相关目录
RUN mkdir ${PENTAHO_HOME} \
    $PENTAHO_HOME/docker-entrypoint.d \
    $PENTAHO_HOME/templates \
    $PENTAHO_HOME/scripts \
    /root/.kettle

# 复制模板文件
COPY carte-*.config.xml $PENTAHO_HOME/templates/
COPY docker-entrypoint.sh $PENTAHO_HOME/scripts/

# 给脚本文件添加执行权限
RUN chmod +x $PENTAHO_HOME/scripts/docker-entrypoint.sh

# 下载kettle
RUN wget --progress=dot:giga \
    http://downloads.sourceforge.net/project/pentaho/Data%20Integration/${PDI_RELEASE}/pdi-ce-${PDI_VERSION}.zip \
    -O /tmp/pdi-ce-${PDI_VERSION}.zip && \
    unzip -q /tmp/pdi-ce-${PDI_VERSION}.zip -d $PENTAHO_HOME && \
    rm /tmp/pdi-ce-${PDI_VERSION}.zip

# 添加kettle环境变量
ENV KETTLE_HOME=$PENTAHO_HOME/data-integration \
    PATH=$KETTLE_HOME:$PATH

# Expose Carte Server
EXPOSE ${CARTE_PORT}

# 工作目录
WORKDIR $KETTLE_HOME

# 运行脚本
ENTRYPOINT ["../scripts/docker-entrypoint.sh"]

# 运行 Carte 以下命令为ENTRYPOINT参数
CMD ["carte.sh", "carte.config.xml"]