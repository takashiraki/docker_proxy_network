# Docker Proxy Network

A Docker-based reverse proxy setup using nginx-proxy and Let's Encrypt for automatic SSL certificate management.

## Overview

This project provides a containerized nginx reverse proxy that automatically routes traffic to Docker containers and manages SSL certificates using Let's Encrypt. It's designed to simplify the deployment of multiple web applications on a single server.

## Features

- Automatic reverse proxy configuration for Docker containers
- Automatic SSL certificate generation and renewal via Let's Encrypt
- Support for custom virtual host configurations
- HTTP Basic Authentication support
- Shared network for container communication

## Prerequisites

- Docker
- Docker Compose

## Installation

1. Clone the repository:
```bash
git clone https://github.com/takashiraki/docker_proxy_network.git
cd docker_proxy_network
```

2. Start the services:
```bash
docker-compose up -d
```

## Usage

### Setting up a web application

To use this proxy with your web application, add the following to your application's docker-compose.yml:

```yaml
version: "3"

services:
  your-app:
    image: your-app-image
    environment:
      - VIRTUAL_HOST=yourdomain.com
      - LETSENCRYPT_HOST=yourdomain.com
      - LETSENCRYPT_EMAIL=your-email@example.com
    networks:
      - my_proxy_network

networks:
  my_proxy_network:
    external: true
```

### Custom Virtual Host Configuration

Place custom nginx configuration files in the `./vhost.d/` directory. The filename should match your domain name.

### HTTP Basic Authentication

To add password protection to a domain, create an htpasswd entry in the `./.htpasswd` file:

```bash
htpasswd -c .htpasswd yourdomain.com username
```

## Directory Structure

- `./certs/` - SSL certificates storage
- `./vhost.d/` - Custom virtual host configurations
- `./.htpasswd` - HTTP Basic Authentication credentials
- `./infra/nginx/` - Custom nginx-proxy build files

## Ports

- `80` - HTTP
- `443` - HTTPS

## Networks

The proxy creates a network named `my_proxy_network` that other containers can join to be proxied.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

If you encounter any issues or have questions, please open an issue on the GitHub repository.