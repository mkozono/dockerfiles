# run a tor relay in a container
#
# Bridge relay:
#   docker run -d \
#     --restart always \
#     -v /var/lib/tor:/var/lib/tor \
#     -p 9001:9001 \
#     --name tor-relay \
#     monozok/tor-relay -f /etc/tor/torrc.bridge
#
# Middle relay:
#   docker run -d \
#     --restart always \
#     -v /var/lib/tor:/var/lib/tor \
#     -p 9001:9001 \
#     --name tor-relay \
#     monozok/tor-relay -f /etc/tor/torrc.middle
#
FROM alpine:latest
MAINTAINER Michael Kozono <mike@kozono.co>

# Note: Tor is only in testing repo -> http://pkgs.alpinelinux.org/packages?package=emacs&repo=all&arch=x86_64
RUN apk update && apk add \
  tor openntpd \
  --update-cache --repository http://dl-3.alpinelinux.org/alpine/edge/testing/ \
  && rm -rf /var/cache/apk/*

# default port to used for incoming Tor connections
# can be changed by changing 'ORPort' in torrc
EXPOSE 9001

# copy in our torrc files
COPY torrc.bridge /etc/tor/torrc.bridge
COPY torrc.middle /etc/tor/torrc.middle
COPY torrc.exit /etc/tor/torrc.exit

# make sure files are owned by tor user
RUN chown -R tor /etc/tor

USER tor

ENTRYPOINT [ "tor" ]
CMD [ "-f", "/etc/tor/torrc.middle" ]
