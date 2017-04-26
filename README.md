This Dockerfile builds an image of gost-2.4-dev20170303. Based on debian:oldstable-slim with the image size of 23MB.

Quick Start
This image uses ENTRYPOINT to run the containers as an executable.
docker run -d -p 8080:8080 mixool/gost -L=ss://chacha20:password@:8080
You can configure the service to run on a port of your choice. Just make sure the port number given to docker is the same as the one given to gost.

Create shadowsocks server and client:
1.shadowsocks server
docker run -d -p 8080:8080 mixool/gost -L=ss://chacha20:password@:8080
2.shadowsocks client
// please note this will share the networking interface with the host
// and -L=:8080 will listen on *ALL* interfaces on this host, 
// use -L=127.0.0.1:8080 if you want to listen on localhost only
docker run -d -p 8080:8080 --net=host mixool/gost -L=:8080 -F=ss://chacha20:password@<server_ip>:8080
then try with cURL:
curl -x 127.0.0.1:8080 https://myip.today

Create http_proxy on loalhost of Mac OS X, tunneling to remote shadowsocks server and make it always restart unless stopped manually：
docker run --restart=unless-stopped -d -p 8080:8080 mixool/gost -L=:8080 -F=ss://chacha20:<password>@<ss_host>:<ss_port>

Deploy in app.arukas.io：
1.8080-TCP  CMD：    -L=:8080                                 useage:chrome+SwitchyOmega HTTPS Endpoint:443
2.8088-UDP  CMD：    -L=http2+kcp://:8088                     useage:gost -L=:8080 -F=http2+kcp://<ip>:<port>
3.Monitor different ports 8080-TCP,8088-UDP,8338-tcp
            CMD：    -L=:8080 -L=http2+kcp://:8088 -L=ss://chacha20:password@8338

For more command line options, refer to the：https://github.com/ginuerzh/gost
