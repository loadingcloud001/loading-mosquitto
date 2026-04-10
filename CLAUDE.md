# loading-mosquitto

MQTT broker for IoT messaging.

## Deploy

**MANDATORY: Use GitHub Actions CI/CD only. Never manual SSH deploy.**

- SSH only for: debugging, reading logs, health checks
- Resource limits: 256M memory, 0.25 CPU
- Log rotation: max-size 10m, max-file 3

## Config

- Ports: 1883 (MQTT), 9001 (WebSocket)
- Auth: username/password or TLS

## Dependencies

- Node-RED (uses MQTT)

## Secrets

Encrypted in dev-standards `.secrets/mosquitto.age`

## Resources

- [Dev-Standards](https://github.com/loadingcloud001/dev-standards)
- [Mosquitto Docs](https://mosquitto.org/documentation/)
