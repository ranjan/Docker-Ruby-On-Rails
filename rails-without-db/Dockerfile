
FROM ruby:2.7.2 AS rails-development

ARG USER_ID
ARG GROUP_ID
ARG USER

RUN addgroup --gid $GROUP_ID $USER
RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID $USER

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg -o /root/yarn-pubkey.gpg && apt-key add /root/yarn-pubkey.gpg
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install -y --no-install-recommends nodejs yarn

ENV INSTALL_PATH /opt/rails_demo_app

WORKDIR $INSTALL_PATH

RUN gem install rails bundler
RUN cd /opt/ && rails new --skip-bundle rails_demo_app

COPY config/Gemfile .
COPY config/unicorn.rb config/
RUN bundle install

RUN rm -rf node_modules vendor
RUN yarn install

RUN chown -R $USER:$USER $INSTALL_PATH

USER $USER
EXPOSE 8010
CMD bundle exec unicorn -c config/unicorn.rb


