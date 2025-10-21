FROM rust:1.82 as builder

# Install dependencies
RUN apt-get update && apt-get install -y \
    pkg-config \
    libssl-dev \
    git \
    curl \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Clone Electrs
RUN git clone https://github.com/romanz/electrs.git /electrs
WORKDIR /electrs

# Build Electrs with REST support
RUN cargo build --release --features=rest

# Runtime image
FROM debian:bullseye-slim
COPY --from=builder /electrs/target/release/electrs /usr/local/bin/electrs
RUN apt-get update && apt-get install -y ca-certificates && rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["/usr/local/bin/electrs"]
