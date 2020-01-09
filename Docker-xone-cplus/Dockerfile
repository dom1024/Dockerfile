FROM centos:centos7.5.1804 as build
LABEL maintainer="<song.wu@xuncetech.com>" \
      name="xone-cplus-dev" \
      author="song.wu" \
      version="v1.0.1" \
      systemver="Centos7.5" \
      desc="xone cplus env" \
      build-date="20200101"
COPY 3rd_src.tar.gz /tmp/3rd_src.tar.gz

RUN set +x && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && yum install epel-release -y \
    && yum install centos-release-scl -y \
    && yum install devtoolset-7-make devtoolset-7-gcc devtoolset-7-gcc-c++ libtool boost-devel openssl-devel file libuv-devel cmake3 -y \
    && scl enable devtoolset-7 bash \
    && echo "source /opt/rh/devtoolset-7/enable" >> /root/.bashrc \
    && source /root/.bashrc \
    && ln -sf /usr/bin/cmake3 /usr/bin/cmake \
    && cd /tmp \
    && tar -zxf 3rd_src.tar.gz && cd 3rd_src \
    && tar -zxf hiredis-0.14.0.tar.gz && cd hiredis-0.14.0 && make -j16 && make install && cd .. \
    && tar -zxf jsoncpp-1.8.0.tar.gz && cd jsoncpp-1.8.0 && cmake -DCMAKE_BUILD_TYPE=release -DBUILD_STATIC_LIBS=ON -DBUILD_SHARED_LIBS=ON -DCMAKE_INSTALL_PREFIX=/usr/ . && make -j16 && make install && cd ..\
    && tar -zxf libzmq-4.2.1.tar.gz && cd libzmq-4.2.1 && ./autogen.sh && ./configure --prefix=/usr && make -j16 && make install && cd .. \
    && tar -zxf protobuf-3.6.1.tar.gz && cd protobuf-3.6.1 && ./autogen.sh && ./configure --prefix=/usr && make -j16 && make install && cd .. \
    && tar -zxf QuantLib-1.16.tar.gz && cd QuantLib-1.16 && ./autogen.sh && ./configure --prefix=/usr && make -j16 && make install && cd .. \
    && tar -zxf spdlog-1.1.0.tar.gz && cd spdlog-1.1.0 && cmake -DCMAKE_INSTALL_PREFIX=/usr . && make -j16 && make install && cd ..\
    && tar -zxf tinyxml2-7.0.0.tar.gz && cd tinyxml2-7.0.0 && cmake -DCMAKE_INSTALL_PREFIX=/usr . && make -j16 && make install && cd ..\
    && tar -zxf uWebSockets-0.14.10.tar.gz && cd uWebSockets-0.14.10 && cmake . && make -j16 && make install && cd ..\
    && tar -zxf xlslib-2.4.0.tar.gz && cd xlslib-2.4.0 && sed -i '80s/CCLD/CXXLD/' ./targets/test/Makefile.in && ./configure --prefix=/usr && make -j16 && make install && cd .. \ 
    && yum install libcurl-devel zlib-devel libuuid-devel -y \
    && rpm -ivh https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm \
    && yum install --enablerepo=mysql80-community --disablerepo=mysql56-community mysql-community-devel -y \ 
    && sed -i '1034s/[<>]/"/g' /usr/include/mysql/mysql_com.h \
    && ldconfig \
    && rm -f /usr/lib/libQuantLib.a \
    && rm -rf /tmp/3rd_src
    

#基础部署环境
FROM centos:centos7.5.1804
LABEL maintainer="<song.wu@xuncetech.com>" \
      name="xone-cplus-deploy" \
      author="song.wu" \
      version="v1.0.1" \
      systemver="Centos7.5" \
      desc="xone cplus env" \
      build-date="20200101"
COPY --from=build /usr/bin/protoc /usr/bin/protoc
COPY --from=build /usr/bin/quantlib-config /usr/bin/quantlib-config
COPY --from=build /usr/bin/quantlib-test-suite /usr/bin/quantlib-config
COPY --from=build /usr/lib /usr/lib
COPY --from=build /usr/lib64 /usr/lib64
#COPY --from=build /etc/uvframe /etc/uvframe
COPY --from=build /etc/ld.so.conf.d /etc/ld.so.conf.d
#COPY --from=build /usr/bin/uvframe /usr/bin/uvframe
#COPY --from=build /usr/lib/systemd/system/uv@.service /usr/lib/systemd/system/uv@.service



RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo '/usr/lib/uvframe' > /etc/ld.so.conf.d/uvframe.conf \
    && mkdir -p /home/frame/logs/ \
    && chmod 777 -R /home/frame/ \
    && ldconfig
    

USER root


CMD ["/usr/sbin/init"]