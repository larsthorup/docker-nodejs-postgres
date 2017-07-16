# Install base software

FROM ubuntu:14.04

RUN apt-get update && apt-get -y install curl

# Install Node.js, see https://github.com/nodesource/distributions

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -

RUN apt-get update && apt-get install -y nodejs

# Install PostgreSQL, see https://docs.docker.com/engine/examples/postgresql_service/#installing-postgresql-on-docker

RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8

RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list

RUN apt-get update && apt-get install -y python-software-properties software-properties-common postgresql-9.6 postgresql-client-9.6 postgresql-contrib-9.6

# Install app

RUN mkdir -p /var/app
COPY . /var/app
RUN cd /var/app && npm install

# Configure PostgreSQL, see https://docs.docker.com/engine/examples/postgresql_service/#installing-postgresql-on-docker

USER postgres

RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/9.6/main/pg_hba.conf

RUN echo "listen_addresses='*'" >> /etc/postgresql/9.6/main/postgresql.conf

EXPOSE 5432
EXPOSE 5050

RUN service postgresql start && psql -c "create database pgdb;" && psql -c "create role pgrole with login password 'pgrole'; grant all privileges on database pgdb to pgrole;"

# Start services

WORKDIR /var/app

CMD service postgresql start && pwd && ls && node --version && node server.js
