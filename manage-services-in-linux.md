# Managing Services in Linux

## Check Service Status
To view the current status of a specific service:
```bash
sudo systemctl status <service_name>
```

## Start a Service
To start a service immediately:
```bash
sudo systemctl start <service_name>
```

## Stop a Service
To stop a running service:
```bash
sudo systemctl stop <service_name>
```

## Restart a Service
To stop and start a service again:
```bash
sudo systemctl restart <service_name>
```

## Reload a Service
To reload the configuration of a service without interrupting the current session:
```bash
sudo systemctl reload <service_name>
```

## Enable a Service
To configure a service to start automatically at boot:
```bash
sudo systemctl enable <service_name>
```

## Disable a Service
To stop a service from starting at boot:
```bash
sudo systemctl disable <service_name>
```

## Check if a Service is Enabled
To verify if a service is configured to start at boot:
```bash
sudo systemctl is-enabled <service_name>
```

---

## Additional Commands

### List All Active Services
To list all active services:
```bash
sudo systemctl list-units --type=service --state=active
```

### List All Failed Services
To display only the failed services:
```bash
sudo systemctl list-units --type=service --state=failed
```

### List All Services (Any State)
To list all services (active, inactive, failed, etc.):
```bash
sudo systemctl list-units --type=service
```

### Mask a Service
To prevent a service from being started (even manually):
```bash
sudo systemctl mask <service_name>
```

### Unmask a Service
To allow a previously masked service to be started again:
```bash
sudo systemctl unmask <service_name>
