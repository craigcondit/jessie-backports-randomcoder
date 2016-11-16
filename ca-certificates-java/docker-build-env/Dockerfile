FROM debian:jessie
MAINTAINER ccondit@randomcoder.org

ADD assets /assets

RUN \
	echo "deb http://ftp.us.debian.org/debian jessie main non-free contrib" > /etc/apt/sources.list && \
	echo 'deb http://httpredir.debian.org/debian jessie-updates main non-free contrib' >> /etc/apt/sources.list && \
	echo "deb-src http://ftp.us.debian.org/debian stretch main contrib non-free" >> /etc/apt/sources.list && \
	apt-key add /assets/bintray-public.key.asc && \
	apt-get update && \
	apt-get -y install default-jdk javahelper junit4 apt-transport-https && \
	echo "deb https://dl.bintray.com/insideo/jessie-backports-randomcoder randomcoder main" >> /etc/apt/sources.list && \
	apt-get update && \
	apt-get -y upgrade openssl ca-certificates && \
	apt-get -y install dpkg-dev build-essential devscripts lintian patch patchutils && \
	rm -rf /var/cache/apt && \
	mkdir -p /build /packages

RUN \
	cd /build && \
	apt-get update && \
	apt-get -y build-dep ca-certificates-java && \
	apt-get -y source ca-certificates-java && \
	rm -rf /var/cache/apt && \
	cd ca-certificates-java-* && \
	DEBEMAIL=ccondit@randomcoder.org DEBFULLNAME="Craig Condit" \
	dch -b -l "~local" --distribution jessie-backports-randomcoder --force-distribution --noquery \
	'Rebuild for Debian Jessie.' && \
	dpkg-buildpackage && \
	cd /build && \
	cp -f *~local*.deb /packages/
