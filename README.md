# loading-mosquitto

MQTT broker deployment for the Loading platform, powered by Eclipse Mosquitto.

## Overview

Mosquitto is a lightweight open-source MQTT broker that enables messaging between IoT devices and consumers. This repository contains the Docker Compose deployment configuration for the Loading platform's MQTT broker.

## Ports

| Port | Service | Protocol |
|------|---------|----------|
| 1883 | MQTT | Default unencrypted MQTT |
| 9001 | WebSocket | For web-based clients |

## Configuration

Configuration files are located in `deploy/config/`:

- `mosquitto.conf` - Main broker configuration (ports, persistence, logging)
- `pwfile` - Password file for authentication (disabled by default)
- `aclfile` - Access control list for topic-based permissions (disabled by default)

### Key Configuration Options

```conf
listener 1883           # MQTT port
listener 9001           # WebSocket port
protocol websockets     # Enable WebSocket support
persistence true        # Enable message persistence
persistence_location /mosquitto/data/
log_dest file /mosquitto/log/mosquitto.log
allow_anonymous true    # Authentication disabled (enable for production)
```

## Deployment

### Prerequisites

- Docker and Docker Compose
- External network `cmp-network`

### Start the Broker

```bash
cd deploy
docker-compose up -d
```

### Stop the Broker

```bash
docker-compose down
```

### View Logs

```bash
docker-compose logs -f mosquitto
```

## Data Persistence

The broker persists data to Docker volumes:

- `deploy/config/` - Configuration files
- `deploy/data/` - Message persistence (mosquitto.db)
- `deploy/log/` - Broker logs

## Security

Authentication is disabled by default (`allow_anonymous true`). To enable:

1. Create a password file:
   ```bash
   docker exec loading-mosquitto mosquitto_passwd -c /mosquitto/config/pwfile <username>
   ```

2. Uncomment the password line in `mosquitto.conf`:
   ```conf
   password_file /mosquitto/config/pwfile
   ```

3. Optionally add an ACL file and uncomment:
   ```conf
   acl_file /mosquitto/config/aclfile
   ```

4. Restart the broker:
   ```bash
   docker-compose restart
   ```

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     Mosquitto                            │
│                  (Docker Compose)                        │
│                                                              │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐  │
│  │  MQTT  │  │   ACL   │  │  Auth   │  │   TLS   │  │
│  │ Broker │  │ Manager │  │ Manager │  │  Support│  │
│  └─────────┘  └─────────┘  └─────────┘  └─────────┘  │
└─────────────────────┬───────────────────────────────────┘
                      │
         ┌────────────┼────────────┐
         ▼            ▼            ▼
┌─────────────┐ ┌───────────┐ ┌─────────┐
│  Node-RED   │ │   IoT     │ │ Other   │
│  (Consumer) │ │ Devices  │ │ Clients │
└─────────────┘ └───────────┘ └─────────┘
```

## Connecting Clients

### MQTT (port 1883)

```bash
mosquitto_pub -h <host> -t "loading/sensors" -m "hello"
mosquitto_sub -h <host> -t "loading/sensors"
```

### WebSocket (port 9001)

Connect using any MQTT.js-compatible client with ws:// or wss:// protocol.

## License

Eclipse Mosquitto is licensed under the Eclipse Public License. This deployment configuration is part of the Loading platform.
