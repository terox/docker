FROM nginx:1.18.0

ARG php_container=php
ARG preset=symfony4.conf

ENV PHP_CONTAINER=${php_container}

COPY nginx.template /tmp/nginx.template
ADD presets /tmp/presets/

# Replace env variables into template file
RUN /bin/bash -c "envsubst < /tmp/nginx.template > /etc/nginx/nginx.conf"

# Copy the preset as a current nginx server
RUN cp "/tmp/presets/${preset}" /etc/nginx/conf.d/default.conf
