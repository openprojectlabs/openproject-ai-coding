ARG RUST_VERSION=1.88
FROM rust:${RUST_VERSION}-slim-bookworm

# Native DBus headers and pkg-config are required by libdbus-sys during
# `cargo build`. Keep these development dependencies in the Dev Container,
# rather than adding them to the production runtime image.
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        git \
        libdbus-1-dev \
        pkg-config \
    && rm -rf /var/lib/apt/lists/*

# The official Rust image keeps its toolchain here. Preserve it after switching
# from the image's root user to the non-root development user below.
ENV PATH=/usr/local/cargo/bin:${PATH}
RUN printf '%s\n' 'export PATH=/usr/local/cargo/bin:$PATH' \
    > /etc/profile.d/openproject-rust.sh
RUN rustup component add rustfmt

RUN groupadd --gid 1000 openproject \
    && useradd --create-home --shell /bin/bash --uid 1000 --gid 1000 openproject \
    && install -d -m 0700 -o openproject -g openproject /home/openproject/.openproject \
    && install -d -m 0755 -o openproject -g openproject /home/openproject/.cargo \
    && install -d -m 0755 -o openproject -g openproject /home/openproject/.cargo/target

USER openproject
WORKDIR /workspaces/CodeWhale
