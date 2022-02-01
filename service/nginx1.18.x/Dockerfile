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
# Note: the sed command only filters the current environment variables to avoid issues with NGINX variables.
#       You can read more:
#         Link 1: https://stackoverflow.com/questions/24963705/is-there-an-escape-character-for-envsubst
#         Link 2: https://stackoverflow.com/questions/24963705/is-there-an-escape-character-for-envsubst#comment103605562_54318029
RUN /bin/bash -c envsubst "$(env | sed -e 's/=.*//' -e 's/^/\$/g')" < /tmp/nginx.template > /etc/nginx/nginx.conf
RUN /bin/bash -c envsubst "$(env | sed -e 's/=.*//' -e 's/^/\$/g')" < /tmp/presets/${preset}.template > /etc/nginx/conf.d/default.conf
