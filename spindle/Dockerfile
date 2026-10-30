FROM rust:1.99.1-slim-trixie AS hydrant
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates build-essential cmake git \
    && rm -rf /var/lib/apt/lists/*
COPY go.mod /go.mod
RUN REV=$(sed -n 's|.*tangled.org/ptr.pet/hydrant v.*-\([0-9a-f]\{12\}\)$|\1|p' /go.mod) \
    && git clone https://tangled.org/ptr.pet/hydrant /src \
    && git -C /src checkout "$REV"
WORKDIR /src
RUN cargo build --release -p hydrant-ffi

FROM golang:1.27.2-trixie AS builder
RUN apt-get update && apt-get install -y --no-install-recommends libsqlite3-dev \
    && rm -rf /var/lib/apt/lists/*
COPY --from=hydrant /src/target/release/libhydrant_ffi.a /usr/local/lib/
WORKDIR /src
COPY . .
RUN CGO_LDFLAGS=-L/usr/local/lib go build -tags libsqlite3 -o /spindle ./cmd/spindle

FROM debian:forky-slim
RUN apt-get update && apt-get install -y --no-install-recommends \
    git ca-certificates libsqlite3-0 \
    && rm -rf /var/lib/apt/lists/*
COPY --from=builder /spindle /usr/local/bin/spindle
ENTRYPOINT ["/usr/local/bin/spindle"]
