# base-image for node on any machine using a template variable,
# see more about dockerfile templates here: http://docs.resin.io/deployment/docker-templates/
# and about resin base images here: http://docs.resin.io/runtime/resin-base-images/
# Note the node:slim image doesn't have node-gyp
FROM resin/%%RESIN_MACHINE_NAME%%-node:slim

# use apt-get if you need to install dependencies,
# for instance if you need ALSA sound utils, just uncomment the lines below.
RUN apt-get update && apt-get install -yq \
    autoconf autogen libtool uthash-dev libjansson-dev libcurl4-openssl-dev libusb-dev libncurses-dev git-core && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

git clone https://github.com/luke-jr/bfgminer.git /usr/src/app/bfgminer

# Defines our working directory in container
WORKDIR /usr/src/app/bfgminer

RUN ./autogen.sh 

RUN ./configure

RUN make

# Enable systemd init system in container
ENV INITSYSTEM on

# server.js will run when container starts up on the device
CMD ["./bfgminer", "-o stratum.bitcoin.cz:3333 -O username.worker:password -S all"]
