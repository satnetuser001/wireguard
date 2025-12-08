FROM ubuntu:latest

RUN apt-get update && \
    apt-get install -y wireguard iproute2 iptables && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

CMD ["wg-quick", "up", "wg0"]
