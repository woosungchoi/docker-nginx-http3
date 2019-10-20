# docker-nginx-http3

Alpine Linux (`edge`) image with nginx with HTTP/3 (QUIC), TLSv1.3, 0-RTT, brotli support. All built on the bleeding edge.

HTTP/3 support provided from the smart people at [CloudFlare](https://cloudflare.com) with the [cloudflare/quiche](https://github.com/cloudflare/quiche) project.

Images for this are available on Docker Hub.

`docker pull ranadeeppolavarapu/nginx-http3`

## Features

- HTTP/3 (QUIC)
- HTTP/2
- HTTP/2 Server Push
- BoringSSL (Google's flavor of OpenSSL)
- TLS 1.3 **with 0-RTT support**
- Brotli compression

## HTTP/3 ENABLED!

Using Chrome Canary with the following CLI flags:

```bash
--flag-switches-begin --enable-quic --quic-version=h3-23 --enable-features=EnableTLS13EarlyData --flag-switches-end
```

Run on Mac OS (**darwin**):

- ```bash
   "/Applications/Google Chrome Canary.app Contents/MacOS/Google Chrome Canary" \
     --flag-switches-begin \
     --enable-quic \
     --quic-version=h3-23 \
     --enable-features=EnableTLS13EarlyData \
     --flag-switches-end
  ```

### HTTP/3 (QUIC) Proof

Since HTTP/3 is experimental, we have to be sensible with it. Therefore, below is HTTP/3 in production on one of my web apps 😄.

![h3](https://user-images.githubusercontent.com/7084995/67162952-831d5800-f337-11e9-9297-05241a693cc4.png)

## HTTP/2 with Server Push

![alt](https://user-images.githubusercontent.com/7084995/67162942-654ff300-f337-11e9-9dc0-6d7a915d517c.png)
