
FROM openjdk:8-jdk-slim

LABEL vendor="Seudev" \
    homepage="http://dockerized-apps.seudev.com" \
    author="Thomás Sousa Silva <thomassousa.dev>"

RUN apt-get update \
    && apt-get install -y libgtk2.0-0 \
    && apt-get install -y libswt-webkit-gtk-4-jni \
    && rm -rf /var/lib/apt/lists/*

ENV JASPERSOFT_STUDIO_VERSION="5.5.1"
ARG JASPERSOFT_STUDIO_PLATAFORM="linux-x86_64"
ARG JASPERSOFT_STUDIO_PACKAGE="jaspersoftstudio-$JASPERSOFT_STUDIO_VERSION.final-$JASPERSOFT_STUDIO_PLATAFORM.tgz"
ARG JASPERSOFT_STUDIO_PACKAGE_URL="https://downloads.sourceforge.net/project/jasperstudio/JaspersoftStudio-$JASPERSOFT_STUDIO_VERSION/$JASPERSOFT_STUDIO_PACKAGE"

ADD $JASPERSOFT_STUDIO_PACKAGE_URL /tmp

ENV JASPERSOFT_STUDIO_HOME=/opt/jaspersoft-studio
ENV PATH=$PATH:$JASPERSOFT_STUDIO_HOME
ENV DISPLAY=:0

WORKDIR $JASPERSOFT_STUDIO_HOME

RUN tar -x -f "/tmp/$JASPERSOFT_STUDIO_PACKAGE" -C /tmp \
    && mv /tmp/jaspersoftstudio-$JASPERSOFT_STUDIO_VERSION.final/* $JASPERSOFT_STUDIO_HOME \
    && rm -rf "/tmp/$JASPERSOFT_STUDIO_PACKAGE" "/tmp/jaspersoftstudio-$JASPERSOFT_STUDIO_VERSION.final" \
    && ln -s "$JASPERSOFT_STUDIO_HOME/Jaspersoft Studio" "$JASPERSOFT_STUDIO_HOME/jaspersoft-studio"

CMD ["jaspersoft-studio"]
