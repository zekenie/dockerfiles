FROM resin/rpi-raspbian
MAINTAINER Zeke Nierenberg <hampzan09@gmail.com>

ENV LANGUAGE en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

RUN apt-get update

RUN apt-get -y --force-yes install locales

RUN locale-gen en_US.UTF-8

RUN DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales

RUN apt-get -y --force-yes install aufs-tools

RUN apt-get update && apt-get -y --force-yes install wget

# ADD sources.list /etc/apt/sources.list.d/peer60-postgres.list

# RUN wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | apt-key add -

RUN apt-get -y --force-yes install adduser

RUN adduser --system --disabled-login --home /var/lib/postgresql --no-create-home \
            --group --gecos "PostgreSQL administrator" --shell /bin/bash \
            --uid 5432 postgres

# RUN apt-get update --no-list-cleanup
RUN DEBIAN_FRONTEND=noninteractive apt-get -y --force-yes install postgresql \
      && rm -rf /var/lib/apt/lists/* \
      && apt-get clean

ADD pg_hba.conf /etc/postgresql/9.3/main/pg_hba.conf
ADD postgresql.conf /etc/postgresql/9.3/main/postgresql.conf

USER postgres

EXPOSE 5432
CMD ["/usr/lib/postgresql/9.3/bin/postgres", \
  "-D", "/var/lib/postgresql/9.3/main", \
  "-c", "config_file=/etc/postgresql/9.3/main/postgresql.conf" \
]
