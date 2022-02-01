FROM nginx:1.18.0

ARG php_container=php
ARG preset=symfony4.conf
ARG public_path=public
ARG domains="example.com www.example.com"

ENV PHP_CONTAINER=${php_container}
ENV PUBLIC_PATH=${public_path}
ENV DOMAINS=${domains}

COPY nginx.template /tmp/nginx.template
ADD presets /tmp/presets/

# Replace env variables into template file
RUN /bin/bash -c "envsubst < /tmp/nginx.template > /etc/nginx/nginx.conf"
RUN /bin/bash -c "envsubst < /tmp/presets/${preset}.template > /etc/nginx/conf.d/default.conf"
