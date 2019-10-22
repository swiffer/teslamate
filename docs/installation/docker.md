# Installation with Docker

The recommended way to install and run TeslaMate is to use Docker. Create a file called `docker-compose.yml` with the following content:

**docker-compose.yml**

```YAML
version: '3'

services:
  teslamate:
    image: teslamate/teslamate:latest
    restart: unless-stopped
    environment:
      - DATABASE_USER=teslamate
      - DATABASE_PASS=secret
      - DATABASE_NAME=teslamate
      - DATABASE_HOST=db
      - MQTT_HOST=mosquitto
    ports:
      - 4000:4000
    cap_drop:
      - all

  db:
    image: postgres:11
    environment:
      - POSTGRES_USER=teslamate
      - POSTGRES_PASSWORD=secret
    volumes:
      - teslamate-db:/var/lib/postgresql/data

  grafana:
    image: teslamate/grafana:latest
    environment:
      - DATABASE_USER=teslamate
      - DATABASE_PASS=secret
      - DATABASE_NAME=teslamate
      - DATABASE_HOST=db
    ports:
      - 3000:3000
    volumes:
      - teslamate-grafana-data:/var/lib/grafana

  mosquitto:
    image: eclipse-mosquitto:1.6
    ports:
      - 1883:1883
    volumes:
      - mosquitto-conf:/mosquitto/config
      - mosquitto-data:/mosquitto/data

volumes:
    teslamate-db:
    teslamate-grafana-data:
    mosquitto-conf:
    mosquitto-data:
```

Afterwards start the stack with `docker-compose up`.

To sign in with your Tesla Account open the web interface at [http://your-ip-address:3000](http://localhost:3000).

The Grafana dashboards are available at [http://your-ip-address:3000](http://localhost:3000).