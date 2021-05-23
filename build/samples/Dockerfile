FROM python:3-alpine

LABEL maintainer="Vadim Pshentsov <pshentsoff@gmail.com>"

RUN echo @edge http://nl.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories \
	&& echo @edge http://nl.alpinelinux.org/alpine/edge/main >> /etc/apk/repositories \
	&& apk update && apk upgrade \
	&& apk --no-cache add \
	    curl \
	    g++ \
	    gcc \
	    linux-headers

ARG BOOST_VERSION="1.76.0"
ARG BOOST_VERSION_SLUG="1_76_0"
RUN curl -L -v -o boost.tar.gz "https://boostorg.jfrog.io/artifactory/main/release/${BOOST_VERSION}/source/boost_${BOOST_VERSION_SLUG}.tar.gz" \
    && tar -xvzf  boost.tar.gz \
    && rm -rf boost.tar.gz
ARG BOOST_INSTALLATION_PATH="/boost"
RUN mv boost_${BOOST_VERSION_SLUG} boost \
    && cd boost \
    && ./bootstrap.sh --with-toolset=gcc \
    && ./b2 install --prefix=${BOOST_INSTALLATION_PATH}

COPY ./requirements.txt /
RUN pip install -U pip \
    && pip install -r /requirements.txt

WORKDIR /var/local/samples

COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
