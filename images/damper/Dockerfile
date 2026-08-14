# Usata la stessa immagine di base per compilare
FROM kathara/core:latest AS damper
RUN apt update && apt install -y --no-install-recommends \
    libnetfilter-queue-dev \
    gcc \
    build-essential && \
    apt clean && rm -rf /tmp/* /var/lib/apt/lists/* /var/tmp/*
WORKDIR /src
RUN git clone --depth 1 https://github.com/vmxdev/damper.git .
RUN gcc -Wall -pedantic damper.c modules.conf.c -o damper -lnetfilter_queue -pthread -lrt -lm

# Immagine kathara/base
FROM kathara/core:latest
LABEL org.opencontainers.image.authors="Kathara Team <contact@kathara.org>"
RUN apt update && apt upgrade -y && apt install -y --no-install-recommends \
	libnetfilter-queue-dev \
  apache2 \
	apache2-bin \
	bind9 \
	bind9-utils \
  dnsmasq && \
	apt clean && rm -rf /tmp/* /var/lib/apt/lists/* /var/tmp/*
RUN ln -s /etc/init.d/named /etc/init.d/bind
COPY apache2.service /usr/lib/systemd/system/apache2.service

RUN mkdir /etc/damper
COPY --from=damper /src/damper /usr/local/bin/damper
COPY --from=damper /src/damper.conf /etc/damper/damper.conf

#Opzionale uno dei due o altra versione
#RUN echo "alias damper='damper /etc/damper/damper.conf &'" >> /root/.bashrc
#RUN echo "alias shaper='damper /etc/damper/damper.conf &'" >> /root/.bashrc
