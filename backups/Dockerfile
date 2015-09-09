FROM ubuntu:trusty
MAINTAINER bmcmanus@gmail.com


RUN apt-get update && \
    apt-get install -yq ntpdate inotify-tools python-paramiko python-gobject-2 python-boto duplicity

VOLUME /var/backup

ADD ./backup.sh /backup.sh
ENTRYPOINT ["/backup.sh"]
