FROM centos:7
MAINTAINER 企业级分布式应用服务EDAS研发团队 <edas-dev@list.alibaba-inc.com>

# 安装打包必备软件
RUN yum install -y wget unzip telnet lsof net-tools bind-utils

# 准备 JDK/Tomcat 系统变量
ENV JAVA_HOME /usr/java/latest
ENV PATH ${JAVA_HOME}/bin:$PATH
ENV ADMIN_HOME /home/admin

# 下载安装 OpenJDK
RUN yum -y install java-1.8.0-openjdk-devel

# 创建 JAVA_HOME 软链接
RUN if [ ! -L "${JAVA_HOME}" ]; then mkdir -p `dirname ${JAVA_HOME}` && ln -s `readlink -f /usr/lib/jvm/java` ${JAVA_HOME}; fi

# 下载部署 EDAS 演示 JAR 包
RUN mkdir -p ${ADMIN_HOME}/app && \
         wget http://edas-hz.oss-cn-hangzhou.aliyuncs.com/demo/1.0/hello-edas-0.0.1-SNAPSHOT.jar -O ${ADMIN_HOME}/app/hello-edas-0.0.1-SNAPSHOT.jar

# 增加容器内中⽂支持
ENV LANG="en_US.UTF-8"

# 增强 Webshell 使⽤体验
ENV TERM=xterm

# 将启动命令写入启动脚本 start.sh
RUN mkdir -p ${ADMIN_HOME}
RUN echo '${JAVA_HOME}/bin/java -jar ${CATALINA_OPTS} ${ADMIN_HOME}/app/hello-edas-0.0.1-SNAPSHOT.jar'> ${ADMIN_HOME}/start.sh && chmod +x ${ADMIN_HOME}/start.sh

WORKDIR ${ADMIN_HOME}

CMD ["/bin/bash", "/home/admin/start.sh"]