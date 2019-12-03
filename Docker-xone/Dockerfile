FROM centos:centos7.6.1810
LABEL maintainer="<wusong1024@gmail.com>" \
      name="xonebuild" \
      author="song.wu" \
      vendor="xunce" \
      version="v0.1" \
      systemver="Centos7.6" \
      desc="xone build" \
      build-date="20191203"   
# SET ENVIRONNEMENT
ENV LANG zh_CN.UTF-8
COPY Resources /tmp/Resources
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime &&\
    yum-config-manager --add-repo http://mirrors.aliyun.com/repo/Centos-7.repo &&\
    yum-config-manager --add-repo http://mirrors.xuncetech.com/xunce/xunce-dev/yum/el7/xunce-dev.repo &&\
    yum install -y epel-release ntp yum-plugin-fastestmirror vim-enhanced ntp wget bash-completion elinks lrzsz unix2dos dos2unix git java-1.8.0-openjdk openssl-devel \
    rpm-build.x86_64 wget sudo vim unix2dos dos2unix which automake autoconf autogen libtool gettext-devel libcurl-devel unixODBC-devel &&\
    yum install centos-release-scl -y  && yum -y --enablerepo=centos-sclo-rh install devtoolset-7 &&\
    scl enable devtoolset-7 bash && echo "source /opt/rh/devtoolset-7/enable" >> /root/.bashrc &&\
    yum install -y --enablerepo=xunce-dev \
		fastdfs-5.11\
		tinyxml2-7.0.0\
		xlslib-2.4.0\
                 cmake-3.12.3 \
                 boost169-1.69 \
                 freetds-R1_00RC5 \
                 jsoncpp-1.8.4 \
		 spdlog-1.1.0\
		 uWebSockets-0.14.10\
                 hiredis-0.14.0 \
                 protobuf-3.6.1 \
                 libzmq-4.2.5 &&\
    mkdir -p /data/rpmbuild/{BUILD,BUILDROOT,RPMS,SOURCES,SPECS,SRPMS} &&\
    mkdir -p /data/jenkins && mkdir -p /data/jenkins_home &&\
    sed -i '/^%_topdir/c%_topdir /data/rpmbuild' /usr/lib/rpm/macros &&\ 
    cp -r /tmp/Resources/agent.jar /usr/bin/agent.jar && \
    cp -r /tmp/Resources/jenkins.sh /usr/local/bin/jenkins.sh && \
    chmod +x /usr/bin/agent.jar && chmod +x /usr/local/bin/jenkins.sh && \
    yum clean all

#EXPOSE 10050
USER root
#VOLUME ["/sys/fs/cgroup"]
WORKDIR /data/jenkins_home
ENTRYPOINT ["/usr/local/bin/jenkins.sh"]
#CMD ["/usr/sbin/init"]
