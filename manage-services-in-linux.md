## Self-Services and `xinetd` Server

### Self-Services
Self-services are services that are typically managed by individual users or applications rather than by the system administrator. These services are usually run as non-root users and do not require system-wide privileges. 

#### Managing Self-Services:
- Start a self-service using the command line:
  ```bash
  ./<service_script>
  ```
- Check logs (if available):
  ```bash
  cat ~/logs/<service_name>.log
  ```

#### Example:
- A personal web server or development server for testing purposes.

### `xinetd` Server
The `xinetd` (eXtended Internet Services Daemon) is a super-server daemon that manages multiple network-based services. It listens for incoming requests for the services it manages and starts the appropriate service when a request is received.

#### Configuration
- Configuration files are stored in:
  ```bash
  /etc/xinetd.d/
  ```
- Global configuration is located at:
  ```bash
  /etc/xinetd.conf
  ```

#### Commands:
- To check the status of `xinetd`:
  ```bash
  sudo systemctl status xinetd
  ```
- To restart `xinetd` after making changes:
  ```bash
  sudo systemctl restart xinetd
  ```

#### Example Configuration File:
```conf
service ftp
{
    disable         = no
    socket_type     = stream
    protocol        = tcp
    wait            = no
    user            = root
    server          = /usr/sbin/vsftpd
    log_on_success  += USERID
    log_on_failure  += HOST
}
```

#### Advantages:
- Centralized management of services.
- On-demand service activation to save resources.

#### Note:
Modern Linux systems have largely replaced `xinetd` with `systemd` socket activation. However, it is still used in certain scenarios.

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
```

### Check Logs for a Service
To view logs of a specific service:
```bash
sudo journalctl -u <service_name>
```

### View Logs in Real-Time
To monitor logs of a specific service in real-time:
```bash
sudo journalctl -u <service_name> -f
```

### Stop All Services Matching a Pattern
To stop all services that match a specific pattern:
```bash
sudo systemctl stop '<pattern>'
```

### Disable All Services Matching a Pattern
To disable all services that match a specific pattern:
```bash
sudo systemctl disable '<pattern>'
```

### Controlling Services
#### Start/Stop Multiple Services
To start or stop multiple services at once:
```bash
sudo systemctl start <service1> <service2> <service3>
```
```bash
sudo systemctl stop <service1> <service2> <service3>
```

#### Restart Multiple Services
To restart multiple services:
```bash
sudo systemctl restart <service1> <service2> <service3>
```

#### Reload All Services
To reload all services:
```bash
sudo systemctl daemon-reload
```

### Reboot the System
To reboot the system safely:
```bash
sudo systemctl reboot
```

### Shutdown the System
To power off the system safely:
```bash
sudo systemctl poweroff
```

---

