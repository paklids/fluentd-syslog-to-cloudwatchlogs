FROM fluent/fluentd:v1.2.1-debian-onbuild

# below RUN includes plugin as examples elasticsearch is not required
# you may customize including plugins as you wish

RUN buildDeps="sudo make gcc g++ libc-dev ruby-dev" \
 && apt-get update \
 && apt-get install -y --no-install-recommends $buildDeps \
 && apt-get install -y build-essential autogen autoconf libtool libgeoip-dev \
 && sudo gem install \
        fluent-plugin-geoip \
 && sudo gem install \
        fluent-plugin-grok-parser \
 && sudo gem install \
        fluent-plugin-cloudwatch-logs \
 && sudo gem sources --clear-all \
 && SUDO_FORCE_REMOVE=yes \
    apt-get purge -y --auto-remove \
                  -o APT::AutoRemove::RecommendsImportant=false \
                  $buildDeps \
 && rm -rf /var/lib/apt/lists/* \
           /home/fluent/.gem/ruby/2.3.0/cache/*.gem
