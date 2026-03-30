# Mosquitto Architecture

## Overview

Mosquitto is an open-source MQTT broker for lightweight messaging between IoT devices.

## Deployment

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

## Ports

| Port | Service | Notes |
|------|---------|-------|
| 1883 | MQTT | Default unencrypted |
| 9001 | WebSocket | For web clients |

## Authentication

| Method | Description |
|--------|-------------|
| Username/Password | Simple authentication |
| TLS Certificates | Encrypted communication |
| ACL | Topic-based access control |

## Configuration Files

| File | Purpose |
|------|---------|
| `config/mosquitto.conf` | Main configuration |
| `config/pwfile` | Password file |
| `config/aclfile` | Access control list |
| `data/` | Persistent storage |
| `log/` | Log files |

## Dependencies

```
Mosquitto (no upstream dependencies)
        │
        └────────→ Node-RED (main consumer)
        └────────→ IoT Devices (publishers)
```

## Backup

- Configuration: Files in config directory
- Data: Persistence to Docker volume
