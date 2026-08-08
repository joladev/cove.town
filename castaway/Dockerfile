FROM busybox:musl
COPY src/index.html /www/index.html
CMD ["httpd", "-f", "-p", "8080", "-h", "/www"]
