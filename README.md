# Description
`k8s-openresty-streaming` is a full-fledged media streaming server with [OpenResty][1] and [rtmp module][3] for [Kubernetes][6]

# Features
* Support RTMP, HLS and MPEG-DASH streaming (thanks to [nginx rtmp module][3])
* On the fly (and free) SSL registration and renewal inside [OpenResty/nginx][1] with [Let's Encrypt][7] (thanks to [lua-resty-auto-ssl][4])
* Integrated GeoIP REST Api (thanks to [Telize][2])
* Lightweight image based on [openresty/openresty:alpine-fat flavor][5]

# Prerequisites
* [Git][8]
* [Docker][9], required for single server deployment only.
* [Kubernetes (aka K8s)][6], required for K8s cluster deployment.

# Usage
### Quickstart with Docker
SSH to your server and run
```bash
git clone https://github.com/duythongle/k8s-openresty-streaming.git  
docker run -dit --name my_streaming_server \
  -p 80:80 \
  -p 443:443 \
  -p 1935:1935 \
  -v ~/k8s-openresty-streaming/conf.d:/usr/local/openresty/nginx/conf/conf.d \
  -v ~/k8s-openresty-streaming/nginx.conf:/usr/local/openresty/nginx/conf/nginx.conf \
  thongld/k8s-openresty-streaming:alpine-fat \
  openresty -g "daemon off;"
```
Then your browser should display OpenResty welcome home page at http://*streaming_server_ip*/
### Quickstart with Kubernetes
*Coming soon...*
```bash
```
Then go to http://streaming_server_ip/ to view openresty welcome home page
### More...
* GeoIP REST API can be access at public location `/api/geoip`: http://*streaming_server_ip*/api/geoip/location/8.8.4.4.
Read more geoip api at [Telize GeoIP REST API][2]
* Point your DNS to the server ip and see the magic of Auto SSL happens at https://*streaming_server_domain*
> Note: Let's Encrypt has [rate limits][10] and the first https request for a domain may take a few seconds to complete
* [nginx rtmp module][3] has default application endpoint `my_live_stream`. You can push your live stream to the server via url:
rtmp://*streaming_server_ip_or_domain*:1935/*my_live_stream*/*my_stream_name*

# Building from source
```bash
git clone https://github.com/duythongle/k8s-openresty-streaming.git
cd k8s-openresty-streaming
docker build -t openresty-streaming-server -f alpine-dev/Dockerfile .
```

# TODO
* 12factor ready
* K8s ready
* Helm support
* Auto reload nginx when .conf changes

[1]: https://github.com/openresty/openresty
[2]: https://raw.githubusercontent.com/fcambus/telize
[3]: https://github.com/arut/nginx-rtmp-module
[4]: https://github.com/GUI/lua-resty-auto-ssl
[5]: https://github.com/openresty/docker-openresty/blob/master/alpine-fat/Dockerfile
[6]: https://kubernetes.io/
[7]: https://letsencrypt.org
[8]: https://git-scm.com/
[9]: https://docker.io
[10]:https://letsencrypt.org/docs/rate-limits/