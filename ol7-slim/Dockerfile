FROM oraclelinux:7-slim

# Install dependencies
RUN yum -y install make gcc cc tar gzip

# Install Redis
RUN \
  cd /tmp && \
#  curl -O http://download.redis.io/redis-stable.tar.gz && \
  curl -O http://download.redis.io/releases/redis-4.0.2.tar.gz && \
  tar xvzf redis-4.0.2.tar.gz && \
#  cd redis-stable && \
  cd redis-4.0.2 && \
  make && \
  make install && \
  cp -f src/redis-sentinel /usr/local/bin && \
  mkdir -p /etc/redis && \
  cp -f *.conf /etc/redis && \
  rm -rf /tmp/redis-4.0.2* && \
  sed -i 's/^\(bind .*\)$/# \1/' /etc/redis/redis.conf && \
  sed -i 's/^\(daemonize .*\)$/# \1/' /etc/redis/redis.conf && \
  sed -i 's/^\(dir .*\)$/# \1\ndir \/data/' /etc/redis/redis.conf && \
  sed -i 's/^\(logfile .*\)$/# \1/' /etc/redis/redis.conf

# Cleanup dependencies
RUN yum -y erase gcc cc tar gzip

# clean the YUM cache.
RUN yum clean all

# Define mountable directories
VOLUME ["/data"]

# Define working directory
WORKDIR /data

# Define entrypoint
ENTRYPOINT ["redis-sentinel", "/etc/redis/sentinel.conf"]

# Expose ports
EXPOSE 26379
