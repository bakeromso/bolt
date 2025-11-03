# Use an official Ubuntu base image
FROM debian:trixie-slim

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends \
    tzdata \
    build-essential \
    cmake \
    gcc-arm-none-eabi \
    libnewlib-arm-none-eabi \
    libstdc++-arm-none-eabi-newlib \
    git \
    ca-certificates \
    python3 
 
RUN rm -rf /var/lib/apt/lists/*

# Add the build script inside the container
COPY build.sh /build.sh
RUN chmod +x /build.sh

# Set working directory inside the container
WORKDIR /project

# Default command to run when starting container (can be overridden)
CMD ["bash"]
