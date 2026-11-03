FROM nixos/nix
ARG NIXERY_REV=76760ad8cadd56841c6c99e620acbfd525c9c9a4
ADD https://github.com/tazjin/nixery/archive/${NIXERY_REV}.tar.gz /src/nixery.tar.gz
WORKDIR /src
RUN tar xf nixery.tar.gz && mv nixery-${NIXERY_REV} nixery
RUN nix-build nixery -A nixery -o /out/nixery
RUN nix-build nixery -A nixery-prepare-image -o /out/prepare-image
ENV PATH="/out/nixery/bin:/out/prepare-image/bin:${PATH}"
EXPOSE 8080
CMD ["/out/nixery/bin/server"]
