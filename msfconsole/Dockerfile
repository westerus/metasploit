FROM alpine

MAINTAINER  Westerus <westerus@gmail.com>

LABEL manteiner="Westerus <westerus@gmail.com>" \
      description="Metasploit remote msfconsole" \
      version="0.1.0"

ARG PKG_FLAGS_COMMON="-q"
ARG PKG_FLAGS_PERSISTANT="${PKG_FLAGS_COMMON} --clean-protected --no-cache"
ARG PKG_FLAGS_DEV="${PKG_FLAGS_COMMON} --no-cache"
ARG PKGUPDATE="apk ${PKG_FLAGS_COMMON} update"
ARG PKGUPGRADE="apk ${PKG_FLAGS_PERSISTANT} upgrade"
ARG DELTEMP="rm -rf /var/tmp/* /tmp/* /usr/src/*"
ARG PKGINSTALL="apk ${PKG_FLAGS_PERSISTANT} add"
ARG PKGREMOVE="apk ${PKG_FLAGS_COMMON} --purge del"

ENV MSFHOST="localhost" MSFPORT="55553"
ENV MSFUSER="metasploit" MSFPASS="M3t4Spl01t_"
ENV URL1="https://github.com/allfro/pymetasploit.git" BRANCH1="master"
ENV URL2="https://github.com/Luis-Hebendanz/msf-remote-console.git" BRANCH2="master"

COPY /rootfs/entry-point.sh /

WORKDIR /usr/src/

RUN  $PKGUPDATE \
  && $PKGUPGRADE \
  && $PKGINSTALL ca-certificates python py-setuptools git \
  && git clone $URL1 && git clone $URL2 /msfconsole \
  && cd pymetasploit && python setup.py install \
  && $PKGREMOVE git py-setuptools \
  && $DELTEMP

WORKDIR /msfconsole

ENTRYPOINT ["/entry-point.sh"]
CMD ["python","Main.py","--ssl","--port","$MSFPORT","--host","$MSFHOST","--user","$MSFUSER","--pass","$MSFPASS"]
