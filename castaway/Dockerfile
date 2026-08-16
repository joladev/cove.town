FROM busybox:musl
COPY src/ /www/
CMD ["httpd", "-f", "-p", "8080", "-h", "/www"]
