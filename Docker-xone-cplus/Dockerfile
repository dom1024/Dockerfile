FROM wusong1024/xone-cplus-dev as build
LABEL maintainer="<song.wu@xuncetech.com>" \
      name="xone-cplus-dev" \
      author="song.wu" \
      version="v1.0.3" \
      systemver="Centos7.5" \
      desc="xone cplus env" \
      build-date="2020317"
COPY XONE /tmp/XONE

RUN set +x && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && ldconfig && source /root/.bashrc \
    && cp /tmp/XONE/3rdparty/hs/libt2sdk.so /usr/lib/ && ldconfig \
    && cd /tmp/XONE/src/cpp/ && cmake . && make -j16 && make install \
    && rm -rf /tmp/src 

#基础部署环境
FROM wusong1024/xone-cplus-deploy
LABEL maintainer="<song.wu@xuncetech.com>" \
      name="xone-cplus-deploy" \
      author="song.wu" \
      version="v1.0.3" \
      systemver="Centos7.5" \
      desc="xone cplus env" \
      build-date="20200317"
COPY --from=build /usr/lib/uvframe /usr/lib/uvframe
COPY --from=build /etc/uvframe /etc/uvframe
COPY --from=build /etc/ld.so.conf.d /etc/ld.so.conf.d
COPY --from=build /usr/bin/uvframe /usr/bin/uvframe
COPY --from=build /usr/lib/systemd/system/uv@.service /usr/lib/systemd/system/uv@.service



RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo '/usr/lib/uvframe' > /etc/ld.so.conf.d/uvframe.conf \
    && mkdir -p /home/frame/logs/ \
    && chmod 777 -R /home/frame/ \
    && ldconfig
    

USER root


CMD ["/usr/sbin/init"]
