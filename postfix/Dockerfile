ARG DEBIAN_RELEASE=trixie
FROM debian:${DEBIAN_RELEASE}-slim

ARG DEBIAN_FRONTEND=noninteractive
ENV LC_ALL C

RUN dpkg-divert --local --rename --add /sbin/initctl \
	&& ln -sf /bin/true /sbin/initctl \
	&& dpkg-divert --local --rename --add /usr/bin/ischroot \
	&& ln -sf /bin/true /usr/bin/ischroot

# Add groups and users before installing Postfix to not break compatibility
RUN groupadd -g 102 postfix \
  && groupadd -g 103 postdrop \
  && useradd -g postfix -u 101 -d /var/spool/postfix -s /usr/sbin/nologin postfix \
  && apt-get update && apt-get install -y --no-install-recommends \
	ca-certificates \
	curl \
	dirmngr \
	dnsutils \
	gnupg \
	libsasl2-modules \
	postfix \
	postfix-pcre \
	sasl2-bin \
	sudo \
	supervisor \
	syslog-ng \
	syslog-ng-core \
	tzdata \
	&& rm -rf /var/lib/apt/lists/* \
	&& touch /etc/default/locale \
  && printf '#!/bin/bash\n/usr/sbin/postconf -c /opt/postfix/conf "$@"' > /usr/local/sbin/postconf \
  && chmod +x /usr/local/sbin/postconf

COPY supervisord.conf \
  /etc/supervisor/
COPY syslog-ng.conf \
  /etc/syslog-ng/
COPY conf/smtpd.conf \
  /etc/sasl2/
COPY postfix.sh \
  /opt/
COPY stop-supervisor.sh \
  /usr/local/sbin/

RUN rm -rf /tmp/* /var/tmp/*

HEALTHCHECK \
  --start-period=10s \
  --interval=10s \
  --timeout=5s \
  CMD ! /usr/lib/postfix/sbin/master -t

COPY docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
