# Automated Network Troubleshooting

A full network automation system that monitors virtual routers, detects faults automatically, runs diagnostic commands and delivers reports to engineers in real time.


## Live demo

| | URL |
|---|---|
| ARGUS dashboard | https://biloliddin131313.github.io/argus/ |
| REST API | https://web-production-4de00.up.railway.app |

## What it does

When a network fault occurs, the system automatically:

1. Detects the fault via SNMP traps or syslog
2. Classifies what type of fault it is
3. Runs the appropriate diagnostic commands on the affected router
4. Sends a notification to the engineer via Mattermost
5. Logs everything for review in the ARGUS dashboard

No human intervention needed between fault detection and diagnostic report delivery.

## Stack

| Component | Tool | Purpose |
|---|---|---|
| Virtual network | ContainerLab + Arista cEOS | 3 routers running BGP, generating real SNMP traps |
| Alert monitoring | OpenNMS | Receives SNMP traps and syslog, correlates events |
| Metrics | Prometheus | Collects ongoing network metrics |
| Automation engine | Python + Flask | REST API that executes runbooks and diagnostics |
| Notifications | Mattermost | Delivers fault alerts to engineers |
| Dashboards | Grafana | Visual network health charts |
| Analyst UI | ARGUS | Custom web dashboard for triggering and monitoring faults |

## Project structure

```
containerlab/        Virtual network topology and router configs
automation/          REST API, runbook engine and diagnostic scripts  
dashboard/           ARGUS analyst web dashboard
monitoring/          Docker Compose stack for all monitoring services
```

## Quick start

### 1. Deploy the virtual network

```bash
cd containerlab
sudo containerlab deploy -t lab.yml
```

### 2. Start the monitoring stack

```bash
cd monitoring
docker compose up -d
```

### 3. Start the REST API

```bash
python3 automation/api.py
```

### 4. Open the ARGUS dashboard

Open `dashboard/index.html` in your browser.

## REST API endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | /api/status | Health check |
| GET | /api/routers | List all routers |
| GET | /api/faults | List fault scenarios and recent log |
| POST | /api/fault/trigger | Trigger a fault |
| POST | /api/fault/restore | Restore a fault |
| POST | /api/diagnostic/run | Run diagnostic commands |
| GET | /api/runbooks | List all runbooks |

## Fault scenarios

| Scenario | Router | SNMP trap |
|---|---|---|
| Interface down | router2 | linkDown |
| BGP neighbourship change | router1 | bgpBackwardTransition |
| Hardware fault | router3 | linkDown with error description |
| Route flap | router3 | BGP UPDATE withdrawal |

## Services and ports

| Service | Port |
|---|---|
| ARGUS REST API | 5001 |
| OpenNMS | 8980 |
| Mattermost | 8065 |
| Grafana | 3001 |
| Prometheus | 9090 |

## Requirements

- Docker and Docker Compose
- ContainerLab 0.73+
- Python 3.12+
- Arista cEOS image imported as `ceos:latest`
- Python packages: flask, flask-cors, requests, netmiko

## Author

Biloliddin Turaev
