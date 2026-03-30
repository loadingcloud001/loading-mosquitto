# loading-mosquitto

## Service Purpose

Mosquitto is an open-source MQTT broker for lightweight messaging between IoT devices.

## Environment

- Deployment: Docker Compose
- Default image: `eclipse-mosquitto`
- Ports: `1883` (MQTT), `9001` (WebSocket)
- Auth: Can set username/password or TLS

## Configuration

### Environment Variables

| Variable | Purpose | Example |
|----------|---------|---------|
| `MOSQUITTO_PORT` | MQTT port | `1883` |
| `MOSQUITTO_WS_PORT` | WebSocket port | `9001` |

### Volumes

- `./config/mosquitto.conf` - Configuration file
- `./data` - Persistent data
- `./log` - Logs

## Dependencies

### Services Used
- None (standalone)

### Services That Depend On It
- Node-RED (IoT flow processing)
- Other services needing MQTT

## Resources

- Dev-Standards: https://github.com/loadingcloud001/dev-standards/blob/main/RESOURCES.md
- Mosquitto Docs: https://mosquitto.org/documentation/

## Secrets

MQTT password file encrypted in dev-standards `.secrets/`:
- `mosquitto.age` - MQTT credentials

## Deployment Checklist

- [ ] Set up MQTT authentication
- [ ] Configure TLS (optional)
- [ ] Test connection
- [ ] Set up Node-RED connection
