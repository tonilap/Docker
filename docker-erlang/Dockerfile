FROM ubuntu:14.04
MAINTAINER Toni <tonilap@gmail.com>

ENV OTP_VERSION 17.4

ADD http://www.erlang.org/download/otp_src_${OTP_VERSION}.tar.gz /tmp/

RUN apt-get update && apt-get install -y make gcc ncurses-dev \
  && cd /tmp/ && tar xf otp_src_${OTP_VERSION}.tar.gz && cd otp_src_${OTP_VERSION} \
  && ./configure && make && make install \
  && cd && rm -rf /tmp/otp_src_${OTP_VERSION}