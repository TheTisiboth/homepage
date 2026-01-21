# Homepage Dashboard for Pi5

Dashboard displaying Pi5 system metrics with graphs and Docker containers via auto-discovery.

## Features

- **Real-time system metrics with graphs**: CPU, RAM, disk, network, temperature, uptime
- **Netdata**: Comprehensive real-time monitoring with beautiful interactive charts
- **Glances integration**: Additional system monitoring with historical charts
- **Docker auto-discovery**: Automatically shows all running containers from Dokploy
- **Persistent volumes**: Config survives container restarts
- **Dokploy compatible**: Uses dokploy-network for integration
- **Dark theme** optimized for readability

## Setup with Dokploy UI

1. Create new compose project in Dokploy UI
2. Point to this repository
3. Deploy
4. Access at `http://your-pi-ip:3001`

Dokploy automatically:
- Creates dokploy-network
- Creates named volumes for Netdata
- Mounts config directory from repo

## Configuration

- `config/settings.yaml` - Dashboard appearance & layout
- `config/widgets.yaml` - System resource widgets
- `config/docker.yaml` - Docker auto-discovery settings
- `config/services.yaml` - Custom services (optional)

## Docker Auto-Discovery

All running containers are automatically discovered and displayed. No manual configuration needed.

To exclude containers from appearing, add label to container:
```yaml
labels:
  - "homepage.hide=true"
```

To customize container display, add labels:
```yaml
labels:
  - "homepage.name=My App"
  - "homepage.description=App description"
  - "homepage.icon=docker.png"
  - "homepage.href=http://localhost:8080"
```

## Services

- **homepage**: Main dashboard (port 3001)
- **netdata**: Real-time performance monitoring (port 19999)
  - Access full Netdata UI at `http://your-pi-ip:19999`
  - 1000+ metrics tracked
  - Interactive charts with 1-second granularity
  - Zero configuration needed
- **glances**: System metrics collector with API for graphs

## Volumes

- `./config`: Bind mount for homepage config (edit directly in repo)
- `netdata-config`: Netdata configuration
- `netdata-lib`: Netdata database/metrics storage
- `netdata-cache`: Netdata cache

## Editing Config

Config files in `./config/` directory are mounted directly. Edit and restart:

```bash
# Edit config files directly
vi config/widgets.yaml
docker restart homepage
```

Or edit through Dokploy UI file manager and restart container.

## Notes

- Homepage runs on port 3001
- Netdata runs on port 19999 (full dashboard access)
- Uses dokploy-network for container integration
- Glances provides detailed metrics with 2-second refresh
- Netdata provides 1000+ metrics with 1-second granularity
- Named volumes persist config/data across container/compose removals
- Requires Docker socket access for container discovery
- Requires host filesystem read access for system metrics
- Both Glances and Netdata run in privileged mode for full system access

## Netdata Features

Netdata tracks everything on your Pi5:
- **System**: CPU, RAM, swap, processes, interrupts, context switches
- **Disk**: I/O, space, inodes, backlog
- **Network**: bandwidth, packets, errors, drops per interface
- **Sensors**: Temperature, voltage, fan speeds
- **Applications**: Docker containers, system services
- **Custom dashboards**: Create your own views
- **Alarms**: Built-in health monitoring

Access at `http://your-pi-ip:19999` for the full interactive dashboard.

## Adjusting Metrics

Modify `config/widgets.yaml` to change:
- Disk partition: `disk:sda1` (change to your partition)
- Network interface: `network:eth0` (change to your interface)
- Temperature sensor: `sensor:cpu_thermal` (check available sensors via Glances)
