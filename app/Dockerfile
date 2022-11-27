ARG RVM_RUBY_VERSIONS="3.1.3"
FROM kingdonb/rvm-ruby3:e9eab721
LABEL maintainer="Kingdon Barrett <kingdon@weave.works>"
ENV APPDIR="/home/${RVM_USER}/app"
# ENV SCHEMA="sqlite.schema"
# ENV STATE="xyzgraph.sqlite"

USER root

# install some convenience utils in case this image gets used as dev
RUN apt-get update && apt-get install -y --no-install-recommends \
  manpages vim-tiny && apt-get clean && rm -rf /var/lib/apt/lists/*

# set the time zone
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends \
  tzdata && apt-get clean && rm -rf /var/lib/apt/lists/*
RUN echo "America/New_York" > /etc/timezone
RUN unlink /etc/localtime
RUN dpkg-reconfigure -f noninteractive tzdata

# RUN mkdir ${APPDIR}
WORKDIR ${APPDIR}

# RVM_USER is permitted to create and update files
RUN chown -R ${RVM_USER} ${APPDIR}
USER ${RVM_USER}
ENV RUBY=3.1.3

# include the ruby-version and Gemfile for bundle install
ADD Gemfile Gemfile.lock .ruby-version ${APPDIR}/
RUN  bash --login -c 'bundle install'

# include the app source code
ADD .   ${APPDIR}
# run a web app (for metrics endpoint and to answer health checks)
CMD  bash --login -c 'bundle exec ruby ./web.rb'
EXPOSE 5000
