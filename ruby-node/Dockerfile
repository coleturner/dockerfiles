# Inspired by thehunt:railsstack
FROM heroku/cedar:14
MAINTAINER Cole Turner <turner.cole@gmail.com>

RUN mkdir -p /app/user
WORKDIR /app/user

ENV RUBY_VERSION 2.4.0
ENV GEM_PATH /app/ruby/bundle/ruby/$RUBY_VERSION
ENV GEM_HOME /app/ruby/bundle/ruby/$RUBY_VERSION

# Install Ruby
RUN mkdir -p /app/ruby/bundle/ruby/$RUBY_VERSION
RUN mkdir -p /app/ruby/ruby-$RUBY_VERSION
RUN curl -s --retry 3 -L https://heroku-buildpack-ruby.s3.amazonaws.com/cedar-14/ruby-$RUBY_VERSION.tgz | tar xz -C /app/ruby/ruby-$RUBY_VERSION
ENV PATH /app/ruby/ruby-$RUBY_VERSION/bin:$PATH

ENV NODE_VERSION 7.7.2

# Install Node
RUN curl -s --retry 3 -L http://s3pository.heroku.com/node/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz | tar xz -C /app/ruby/
RUN mv /app/ruby/node-v$NODE_VERSION-linux-x64 /app/ruby/node-$NODE_VERSION
ENV PATH /app/ruby/node-$NODE_VERSION/bin:$PATH

# Install YARN
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
  echo "deb http://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN \
  apt-get update && \
  apt-get install -y vim nano git wget libfreetype6 libfontconfig bzip2 yarn && \
  apt-get autoremove -y && \
  apt-get clean all

# Install Bundler
RUN gem install bundler -v 1.14.6 --no-ri --no-rdoc
ENV PATH /app/user/bin:/app/ruby/bundle/ruby/$RUBY_VERSION/bin:$PATH
ENV BUNDLE_APP_CONFIG /app/ruby/.bundle/config

# export env vars during run time
RUN mkdir -p /app/.profile.d/
RUN echo "cd /app/user/" > /app/.profile.d/home.sh
ONBUILD RUN echo "export PATH=\"$PATH\" GEM_PATH=\"$GEM_PATH\" GEM_HOME=\"$GEM_HOME\" BUNDLE_APP_CONFIG=\"$BUNDLE_APP_CONFIG\"" > /app/.profile.d/ruby.sh

RUN mkdir -p /app/docker/
COPY ./init.sh /usr/bin/init.sh
RUN chmod +x /usr/bin/init.sh

EXPOSE 3000
WORKDIR /app/user