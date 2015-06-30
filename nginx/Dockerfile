FROM nginx:1.7

COPY nginx.conf /etc/nginx/nginx.conf
COPY registry.conf /etc/nginx/conf.d/registry.conf
COPY docker-registry.conf /etc/nginx/docker-registry.conf
COPY docker-registry-v2.conf /etc/nginx/docker-registry-v2.conf
COPY test.crt /etc/nginx/test.crt
COPY test.key /etc/nginx/test.key
COPY .htpasswd /etc/nginx/.htpasswd
