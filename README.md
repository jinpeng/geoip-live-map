# geoip-live-map
Realtime visualization of your access logs on a map

It tails an access log, searches for an IP address in each line, determines it's geo coordinates and 
animates a point on the map in the corresponding location.

![geoip-live-map](https://media.giphy.com/media/xUPGGeuxt7c3wsYODC/giphy.gif)

## Usage
```
docker run -d \
    --name geoip-live-map \
    -e LOG_FILENAME=/var/log/nginx/access.log \
    -e IP_REGEXP="\d+\.\d+\.\d+\.\d+" \
    -e HTTP_LISTEN_ON=:8080 \
    -v /var/log/nginx:/var/log/nginx:ro \
    -p 8080:8080 \
    jinpeng/geoip-live-map
```

## Compile

### Run without Docker
```
go build .
wget http://geolite.maxmind.com/download/geoip/database/GeoLite2-City.tar.gz
tar -zxf GeoLite2-City.tar.gz
mv GeoLite2-City_xxxxxx/GeoLite2-City.mmdb .
LOG_FILENAME=/var/log/nginx/access.log IP_REGEXP="\d+\.\d+\.\d+\.\d+" HTTP_LISTEN_ON=:8080 ./geoip-live-map
```

Open another console, to test it.

```
siege -c 1 -r 1000 -d 10 http://host:port/
```

### Cross compile for Linux (from MacOS)
```
CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -ldflags "-s -w" -o /tmp/geoip-live-map
```
