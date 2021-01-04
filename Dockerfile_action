FROM nginx:1.19.3
LABEL maintainer="Cuyu Tang me@expoli.tech"

# Install wget and install/updates certificates
RUN apt-get update \
 && apt-get install -y -q --no-install-recommends \
    ca-certificates \
    wget \
 && apt-get clean \
 && rm -r /var/lib/apt/lists/*


# Configure Nginx and apply fix for very long server names
RUN echo "daemon off;" >> /etc/nginx/nginx.conf \
 && sed -i 's/worker_processes  1/worker_processes  auto/' /etc/nginx/nginx.conf

# Install Forego
ADD ./forego /usr/local/bin/forego
RUN chmod u+x /usr/local/bin/forego

ENV DOCKER_GEN_VERSION 0.7.4

ADD ./docker-gen /usr/local/bin/docker-gen 
RUN chmod u+x /usr/local/bin/docker-gen

COPY network_internal.conf /etc/nginx/

COPY . /app/
WORKDIR /app/

ENV DOCKER_HOST unix:///tmp/docker.sock

VOLUME ["/etc/nginx/certs", "/etc/nginx/dhparam"]

ENTRYPOINT ["/app/docker-entrypoint.sh"]
CMD ["forego", "start", "-r"]