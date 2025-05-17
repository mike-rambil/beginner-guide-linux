> Source: https: //last9.io/blog/systemctl-guide/

Here's the content you provided converted into properly formatted **Markdown**:

---

# systemctl Guide: Managing Services in Linux

Linux system administrators frequently rely on `systemctl`, a command-line tool used to manage `systemd` services, units, and the system itself. As `systemd` has become the standard init system for most modern Linux distributions, understanding `systemctl` is essential for controlling services, checking system status, and performing system-level tasks. This guide provides a comprehensive overview of `systemctl`, including its commands, use cases, and practical examples to help you effectively manage Linux systems.

---

## What is `systemctl`?

`systemctl` is the primary command-line interface for interacting with `systemd`, the system and service manager for Linux operating systems. `systemd` is responsible for initializing the system, managing services, and handling system resources during runtime.

`systemctl` allows users to:

* Start, stop, restart, enable, disable, and inspect `systemd` units, such as services, sockets, timers, and more.

---

## Key Features of `systemctl`

* **Service Management**: Start, stop, restart, or reload services.
* **Unit Inspection**: View the status and properties of `systemd` units.
* **System Control**: Reboot, power off, or suspend the system.
* **Dependency Management**: Manage dependencies between units.
* **Logging Integration**: Access service logs via `journalctl`.

---

## Understanding `systemd` Units

`systemd` organizes tasks into **units**, which are configuration files defining resources or services. Each unit has a specific type, such as:

* **Service units (`.service`)**: Manage daemons or background processes (e.g., `nginx.service`)
* **Socket units (`.socket`)**: Handle network or IPC sockets
* **Timer units (`.timer`)**: Schedule tasks, similar to cron jobs
* **Mount units (`.mount`)**: Define filesystem mount points
* **Target units (`.target`)**: Group units for system states (e.g., `multi-user.target`)

`systemctl` commands operate on these units to control their behavior.

---

## Basic `systemctl` Commands

### Managing Services

These commands control the runtime state of services.

* **Start a service**

  ```bash
  sudo systemctl start <service>
  ```

  *Example*:

  ```bash
  sudo systemctl start apache2
  ```

* **Stop a service**

  ```bash
  sudo systemctl stop <service>
  ```

  *Example*:

  ```bash
  sudo systemctl stop apache2
  ```

* **Restart a service**

  ```bash
  sudo systemctl restart <service>
  ```

  *Example*:

  ```bash
  sudo systemctl restart apache2
  ```

* **Reload a service**
  Reloads configuration without interrupting the service (if supported).

  ```bash
  sudo systemctl reload <service>
  ```

  *Example*:

  ```bash
  sudo systemctl reload apache2
  ```

* **Check service status**

  ```bash
  systemctl status <service>
  ```

  *Example*:

  ```bash
  systemctl status apache2
  ```

---

### Enabling and Disabling Services

These commands control whether a service starts automatically at boot.

* **Enable a service**

  ```bash
  sudo systemctl enable <service>
  ```

  *Example*:

  ```bash
  sudo systemctl enable apache2
  ```

* **Disable a service**

  ```bash
  sudo systemctl disable <service>
  ```

  *Example*:

  ```bash
  sudo systemctl disable apache2
  ```

* **Check if a service is enabled**

  ```bash
  systemctl is-enabled <service>
  ```

  *Example*:

  ```bash
  systemctl is-enabled apache2
  ```

---

## System State Management

`systemctl` can also control the system’s power state or runlevel.

* **Reboot the system**

  ```bash
  sudo systemctl reboot
  ```

* **Power off the system**

  ```bash
  sudo systemctl poweroff
  ```

* **Suspend the system**

  ```bash
  sudo systemctl suspend
  ```

* **Switch to a different target (runlevel)**

  ```bash
  sudo systemctl isolate <target>
  ```

  *Example*:

  ```bash
  sudo systemctl isolate multi-user.target
  ```

---

## Listing Units

These commands help inspect available units:

* **List all active units**

  ```bash
  systemctl list-units
  ```

* **List all installed services**

  ```bash
  systemctl list-unit-files --type=service
  ```

* **List failed units**

  ```bash
  systemctl --failed
  ```

---

## Managing Unit Files

Unit files define the configuration for `systemd` units. You can view or edit them manually.

* **View a unit file**

  ```bash
  systemctl cat <service>
  ```

  *Example*:

  ```bash
  systemctl cat apache2
  ```

* **Edit a unit file**
  Creates an override file to customize a unit without modifying the original.

  ```bash
  sudo systemctl edit <service>
  ```

* **Reload systemd configuration**
  After modifying unit files:

  ```bash
  sudo systemctl daemon-reload
  ```

---

## Practical Examples

### Example 1: Managing an Nginx Web Server

```bash
sudo systemctl start nginx
systemctl status nginx
sudo systemctl enable nginx
sudo systemctl reload nginx
```

### Example 2: Troubleshooting a Failed Service

If a service fails to start, use:

```bash
systemctl status sshd
journalctl -u sshd -b
```

### Example 3: Creating a Custom Service

1. Create a unit file `/etc/systemd/system/myscript.service`:

```ini
[Unit]
Description=My Custom Script
After=network.target

[Service]
ExecStart=/usr/bin/myscript.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

2. Reload systemd:

```bash
sudo systemctl daemon-reload
```

3. Enable and start the service:

```bash
sudo systemctl enable myscript
sudo systemctl start myscript
```

---

## Advanced `systemctl` Features

### Masking and Unmasking Services

* **Mask a service**

  ```bash
  sudo systemctl mask <service>
  ```

  *Example*:

  ```bash
  sudo systemctl mask apache2
  ```

* **Unmask a service**

  ```bash
  sudo systemctl unmask <service>
  ```

---

## Analyzing Boot Performance

* **Analyze system boot time**

  ```bash
  systemd-analyze
  ```

* **List services by initialization time**

  ```bash
  systemd-analyze blame
  ```

---

## Viewing Dependencies

* **View a service’s dependencies**

  ```bash
  systemctl list-dependencies <service>
  ```

  *Example*:

  ```bash
  systemctl list-dependencies nginx
  ```

---

## Best Practices for Using `systemctl`

* ✅ **Check Status Regularly**: Use `systemctl status` to monitor service health.
* ✅ **Use Reload When Possible**: Prefer `reload` over `restart` to minimize downtime.
* ✅ **Test Configuration Changes**: After editing unit files, use `systemctl daemon-reload`.
* ✅ **Leverage Journalctl for Logs**: Combine `systemctl` with `journalctl`.
* 🚫 **Avoid Masking Critical Services**: Be cautious when masking essential system services.

---

## Common `systemctl` Errors and Fixes

* **Error**: “Unit not found”

  * Ensure the service name is correct and the unit file exists.
  * Check available services with:

    ```bash
    systemctl list-unit-files
    ```

* **Error**: “Failed to start service”

  * Check logs:

    ```bash
    journalctl -u <service>
    ```

* **Error**: “Operation not permitted”

  * Ensure you’re using `sudo` for privileged operations.

---

## Conclusion

`systemctl` is a powerful and versatile tool for managing `systemd` units and controlling Linux systems. By mastering its commands, you can efficiently manage services, troubleshoot issues, and optimize system performance.

Whether you’re starting a web server, scheduling tasks, or analyzing boot performance, `systemctl` provides the flexibility and control needed for effective system administration.

📚 For further reading, consult the official systemd documentation or use the `man systemctl` command.

---

