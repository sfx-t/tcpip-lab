# TCP/IP Lab

Docker-based experimental environment for learning and debugging TCP/IP networks.

## Overview

This project provides an environment to learn and observe TCP/IP network behavior using Docker Compose to start three containers.

- **Server**: Runs Python standard HTTP server
- **Client**: Container with a complete set of network tools
- **Sniffer**: Can capture packets in the same network namespace as the server

## Requirements

- Docker
- Docker Compose

## Usage

### Starting the Environment

```bash
docker-compose up -d
```

### Connecting to Containers

Connect to the client container:
```bash
docker-compose exec client sh
```

Connect to the sniffer container:
```bash
docker-compose exec sniffer sh
```

### Accessing the HTTP Server

From within the client container:
```bash
curl http://server:8000
```

### Packet Capture

Within the sniffer container:
```bash
tcpdump -i eth0 -n
```

### Stopping the Environment

```bash
docker-compose down
```

## Architecture

- All containers are connected via a bridge network called `labnet`
- The sniffer container shares the same network namespace as the server (`network_mode: "service:server"`)
- The sniffer is granted `NET_ADMIN` and `NET_RAW` capabilities

## Provided Web Page

A simple HTML page is included in `site/index.html` and served through the HTTP server.