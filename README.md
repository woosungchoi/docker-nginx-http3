# docker-nginx-brotli

[![Docker Pulls](https://img.shields.io/docker/pulls/ranadeeppolavarapu/nginx-http3?color=brightgreen)](https://hub.docker.com/r/ranadeeppolavarapu/nginx-http3)
![GitHub](https://img.shields.io/github/license/RanadeepPolavarapu/docker-nginx-http3)
[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-v2.0%20adopted-ff69b4.svg)](code_of_conduct.md)  
[![Arch](https://img.shields.io/badge/docker%20arch-linux%2Famd64-blue)](https://hub.docker.com/r/ranadeeppolavarapu/nginx-http3/tags)  
[![Arch](https://img.shields.io/badge/docker%20arch-linux%2Farm64-blue)](https://hub.docker.com/r/ranadeeppolavarapu/nginx-http3/tags)  
[![Arch](https://img.shields.io/badge/docker%20arch-linux%2Farm%2Fv7-blue)](https://hub.docker.com/r/ranadeeppolavarapu/nginx-http3/tags)  
[![Arch](https://img.shields.io/badge/docker%20arch-linux%2Farm%2Fv6-blue)](https://hub.docker.com/r/ranadeeppolavarapu/nginx-http3/tags)

Alpine Linux image with nginx `latest` with, TLSv1.3, 0-RTT,
brotli, NJS, Cookie-Flag support. All built on the bleeding edge. Built on the
edge, for the edge.

Images for this are available on
[Docker Hub](https://hub.docker.com/r/ranadeeppolavarapu/nginx-http3) and
[GHCR](https://github.com/RanadeepPolavarapu/docker-nginx-http3/pkgs/container/nginx-http3).

## Usage

**Docker Hub:** `docker pull ranadeeppolavarapu/nginx-http3`

**GitHub Container Registry (GHCR):**
`docker pull ghcr.io/ranadeeppolavarapu/nginx-http3`

This is a base image like the default _nginx_ image. It is meant to be used as a
drop-in replacement for the nginx base image.

Best practice example Nginx configs are available in this repo. See
[_nginx.conf_](nginx.conf) and [_h3.nginx.conf_](h3.nginx.conf).

Example:

```Dockerfile
# Base Nginx HTTP/3 Image
FROM ranadeeppolavarapu/nginx-http3:latest

# Copy your certs.
COPY localhost.key /etc/ssl/private/
COPY localhost.pem /etc/ssl/

# Copy your configs.
COPY nginx.conf /etc/nginx/
COPY h3.nginx.conf /etc/nginx/conf.d/
```

H3 runs over UDP so, you will need to port map both TCP and UDP. Ex:
`docker run -p 80:80 -p 443:443/tcp -p 443:443/udp ...`

**NOTE**: Please note that you need a valid
[CA](https://en.wikipedia.org/wiki/Certificate_authority) signed certificate for
the client to upgrade you to HTTP/3. [Let's Encrypt](https://letsencrypt.org/)
is a option for getting a free valid CA signed certificate.

## Contributing

Contributions are welcome. Please feel free to contribute 😊.

## Features

- HTTP/2 (with Server Push)
- HTTP/2
- BoringSSL (Google's flavor of OpenSSL)
- TLS 1.3 **with 0-RTT support**
- Brotli compression
- [headers-more-nginx-module](https://github.com/openresty/headers-more-nginx-module)
- [NJS](https://www.nginx.com/blog/introduction-nginscript/)
- [nginx_cookie_flag_module](https://www.nginx.com/products/nginx/modules/cookie-flag/)
- PCRE latest with
  [JIT compilation](http://nginx.org/en/docs/ngx_core_module.html#pcre_jit)
  enabled
- zlib latest
- Alpine Linux (total size of **10 MB** compressed)

## HTTP/2 with Server Push

![alt](https://user-images.githubusercontent.com/7084995/67162942-654ff300-f337-11e9-9dc0-6d7a915d517c.png)

## TLS v1.3

![ssllabs](https://user-images.githubusercontent.com/7084995/67164526-89b4cb00-f349-11e9-87a2-d2dc81610ed4.png)

### 0-RTT Proof

![tls-0-rtt](https://user-images.githubusercontent.com/7084995/67163692-08a50600-f340-11e9-830c-c8a11c824a1f.png)

### Testing 0-RTT

```bash
host=domain.example.com # Replace your domain.
echo -e "GET / HTTP/1.1\r\nHost: $host\r\nConnection: close\r\n\r\n" > request.txt
openssl s_client -connect $host:443 -tls1_3 -sess_out session.pem -ign_eof < request.txt
openssl s_client -connect $host:443 -tls1_3 -sess_in session.pem -early_data request.txt
```
