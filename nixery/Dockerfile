FROM nixos/nix AS build
ARG NIXERY_REV=master
ADD https://github.com/tazjin/nixery/archive/${NIXERY_REV}.tar.gz /src/nixery.tar.gz
WORKDIR /src
RUN tar xf nixery.tar.gz && mv nixery-${NIXERY_REV} nixery
RUN nix-build nixery -A nixery -o /out/nixery
RUN nix-build nixery -A nixery-prepare-image -o /out/prepare-image

FROM nixos/nix
COPY --from=build /out/nixery/ /nixery/
COPY --from=build /out/prepare-image/ /prepare-image/
ENV PATH="/nixery/bin:/prepare-image/bin:${PATH}"
EXPOSE 8080
CMD ["/nixery/bin/server"]
