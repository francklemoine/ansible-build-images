FROM resin/rpi-raspbian
MAINTAINER franck.lemoine@flem.fr

# properly setup debian sources
ENV DEBIAN_FRONTEND=noninteractive

ENV DOMOTICZ_URL=https://releases.domoticz.com/releases/release/domoticz_linux_armv7l.tgz
ENV DOMOTICZ_HOME=/opt/domoticz
ENV PATH ${DOMOTICZ_HOME}:$PATH

RUN buildDeps=' \
		cmake \
		apt-utils \
		build-essential \
		libboost-dev \
		libboost-thread-dev \
		libboost-system-dev \
		libsqlite3-dev \
		subversion \
		curl \
		libcurl4-openssl-dev \
		libusb-dev \
		zlib1g-dev \
		python3-dev \
		iputils-ping \
		wget \
		unzip \
	' \
	set -x \
	&& apt-get -y update \
	&& apt-get -y dist-upgrade \
	&& apt-get install -y --no-install-recommends $buildDeps \
	&& mkdir -p ${DOMOTICZ_HOME}/backups \
	&& mkdir -p ${DOMOTICZ_HOME}/db \
	&& curl -L0k ${DOMOTICZ_URL} -o ${DOMOTICZ_HOME}/domoticz.tgz \
	&& tar xf ${DOMOTICZ_HOME}/domoticz.tgz -C ${DOMOTICZ_HOME} \
	&& rm -f ${DOMOTICZ_HOME}/domoticz.tgz \
	&& chown -R www-data:www-data ${DOMOTICZ_HOME} \
	&& apt-get autoremove \
	&& apt-get clean autoclean \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /tmp/* /var/tmp/*

EXPOSE 8080

# run App as user
USER www-data

WORKDIR ${DOMOTICZ_HOME}

CMD ["domoticz", "-www", "8080", "-dbase" ,"./db/domoticz.db"]
