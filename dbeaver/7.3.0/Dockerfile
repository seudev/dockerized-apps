
FROM openjdk:11-jre-slim

LABEL vendor="Seudev" \
    homepage="http://dockerized-apps.seudev.com" \
    author="Thomás Sousa Silva <thomassousa.dev>"

RUN apt-get update \
    && apt-get install -y libswt-webkit-gtk-4-jni \
    && rm -rf /var/lib/apt/lists/*

ARG DBEAVER_VERSION="7.3.0"
ARG DBEAVER_PLATAFORM="linux.gtk.x86_64"
ARG DBEAVER_PACKAGE="dbeaver-ce-$DBEAVER_VERSION-$DBEAVER_PLATAFORM.tar.gz"
ARG DBEAVER_PACKAGE_URL="https://download.dbeaver.com/community/$DBEAVER_VERSION/$DBEAVER_PACKAGE"

ADD $DBEAVER_PACKAGE_URL /tmp

ENV DBEAVER_HOME=/opt/dbeaver
ENV PATH=$PATH:$DBEAVER_HOME
ENV DISPLAY=:0

WORKDIR $DBEAVER_HOME

RUN tar -x -f "/tmp/$DBEAVER_PACKAGE" -C /opt \
    && rm -rf "/tmp/$DBEAVER_PACKAGE"

CMD ["dbeaver"]
