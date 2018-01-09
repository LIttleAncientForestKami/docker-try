# Dockerfile should start from Docker directive, comment or FROM
# If you do NOT start from a directive, you can't use directive, just FYI.
# My suggestion - take this file line by line, copy&paste and create new image
# Each line should increase the version. Try double-tagging:
# docker build -t devbox:v0.1 -t devbox:latest .
FROM ubuntu
MAINTAINER myself (my@email.com)
# good practiceS here (plural, yes):
# 1. update and install on one line (cache-busting)
# 2. packages installed on separate lines (readability)
# 3. all in one line (for Docker), because of \ (cause Docker layers)
# 4. removing APT lists saves space
RUN apt-get update && apt-get install -y \
  curl \
  unzip \
  zip \
  git \
  inxi \
&& rm -rf /var/lib/apt/lists/*

RUN inxi

# v0.5
# Installing SDKMAN. We need curl, zip and unzip, above. 
RUN curl -s "https://get.sdkman.io" | bash 

ENV SDKMAN_DIR /usr/local/sdkman

RUN set -x \
    && echo "sdkman_auto_answer=true" > $SDKMAN_DIR/etc/config \
    && echo "sdkman_auto_selfupdate=false" >> $SDKMAN_DIR/etc/config \
    && echo "sdkman_insecure_ssl=false" >> $SDKMAN_DIR/etc/config

WORKDIR $SDKMAN_DIR

COPY entrypoint.sh /

ENTRYPOINT ["/entrypoint.sh"]
