# Proxy

## Config .env

evertyhing service `HTTP_PORT` is set to `80`
```json
HTTP_PORT=80
```

how to config `#DB POSTGRES`
```json
DB_HOST=dbpostgres
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=postgres
```

use `http://auth` for `BASEURL_AUTH_SERVICE=`
```json
BASEURL_AUTH_SERVICE=http://auth
```

use `http://otp` for `BASEURL_OTP_SERVICE=`
```json
BASEURL_OTP_SERVICE=http://otp
```

## Prepare

Please make sure port 80 on your host is not used by other services

## How to start

```bash
cd infra/proxy
```

build first
```bash
docker-compose up --build
```

```bash
docker-compose up -d
```
stop service
```bash
docker-compose down
```

## How to access docker postgresql ?

`HOST = localhost`

`PORT = 6543`

`USERNAME = postgres`

`PASSWORD = postgres`

`DATABASE = nabati`