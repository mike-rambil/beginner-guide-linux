> Source: https: //last9.io/blog/systemctl-guide/

systemctl Guide: Managing Services in Linux
Linux system administrators frequently rely on systemctl, a command-line tool used to manage systemd services, units, and the system itself. As systemd has become the standard init system for most modern Linux distributions, understanding systemctl is essential for controlling services, checking system status, and performing system-level tasks. This guide provides a comprehensive overview of systemctl, including its commands, use cases, and practical examples to help you effectively manage Linux systems.
What is systemctl?
systemctl is the primary command-line interface for interacting with systemd, the system and service manager for Linux operating systems. systemd is responsible for initializing the system, managing services, and handling system resources during runtime. systemctl allows users to start, stop, restart, enable, disable, and inspect systemd units, such as services, sockets, timers, and more.
Key Features of systemctl

Service Management: Start, stop, restart, or reload services.
Unit Inspection: View the status and properties of systemd units.
System Control: Reboot, power off, or suspend the system.
Dependency Management: Manage dependencies between units.
Logging Integration: Access service logs via journalctl.

Understanding systemd Units
systemd organizes tasks into units, which are configuration files defining resources or services. Each unit has a specific type, such as:

Service units (.service): Manage daemons or background processes (e.g., nginx.service).
Socket units (.socket): Handle network or IPC sockets.
Timer units (.timer): Schedule tasks, similar to cron jobs.
Mount units (.mount): Define filesystem mount points.
Target units (.target): Group units for system states (e.g., multi-user.target).

systemctl commands operate on these units to control their behavior.
Basic systemctl Commands
Below are some of the most commonly used systemctl commands for managing services and system states.
1. Managing Services
These commands control the runtime state of services.

Start a service:
sudo systemctl start <service-name>

Example: Start the Apache web server.
sudo systemctl start apache2


Stop a service:
sudo systemctl stop <service-name>

Example: Stop the Apache web server.
sudo systemctl stop apache2


Restart a service:
sudo systemctl restart <service-name>

Example: Restart the Apache web server.
sudo systemctl restart apache2


Reload a service:Reloads configuration without interrupting the service (if supported).
sudo systemctl reload <service-name>

Example: Reload the Apache configuration.
sudo systemctl reload apache2


Check service status:Displays the current status of a service, including whether it’s running, stopped, or failed.
systemctl status <service-name>

Example: Check the status of the Apache web server.
systemctl status apache2



2. Enabling and Disabling Services
These commands control whether a service starts automatically at boot.

Enable a service:Configures a service to start at system boot.
sudo systemctl enable <service-name>

Example: Enable Apache to start on boot.
sudo systemctl enable apache2


Disable a service:Prevents a service from starting at boot.
sudo systemctl disable <service-name>

Example: Disable Apache from starting on boot.
sudo systemctl disable apache2


Check if a service is enabled:
systemctl is-enabled <service-name>

Example:
systemctl is-enabled apache2



3. System State Management
systemctl can also control the system’s power state or runlevel.

Reboot the system:
sudo systemctl reboot


Power off the system:
sudo systemctl poweroff


Suspend the system:
sudo systemctl suspend


Switch to a different target (runlevel):
sudo systemctl isolate <target-name>

Example: Switch to multi-user mode (non-graphical).
sudo systemctl isolate multi-user.target



4. Listing Units
These commands help inspect available units.

List all active units:
systemctl list-units


List all installed services:
systemctl list-unit-files --type=service


List failed units:
systemctl --failed



5. Managing Unit Files
Unit files define the configuration for systemd units. You can view or edit them manually.

View a unit file:
systemctl cat <service-name>

Example:
systemctl cat apache2


Edit a unit file:Creates an override file to customize a unit without modifying the original.
sudo systemctl edit <service-name>


Reload systemd configuration:After modifying unit files, reload systemd to apply changes.
sudo systemctl daemon-reload



Practical Examples
Here are real-world scenarios demonstrating systemctl usage.
Example 1: Managing an Nginx Web Server

Start the Nginx service:sudo systemctl start nginx


Check its status:systemctl status nginx


Enable Nginx to start on boot:sudo systemctl enable nginx


Reload Nginx after configuration changes:sudo systemctl reload nginx



Example 2: Troubleshooting a Failed Service
If a service fails to start, use the status command to investigate:
systemctl status sshd

This shows the service’s status, recent logs, and error messages. To view detailed logs, use journalctl:
journalctl -u sshd -b

Example 3: Creating a Custom Service
You can create a custom systemd service by defining a unit file in /etc/systemd/system/. For example, to run a custom script:

Create a unit file (e.g., /etc/systemd/system/myscript.service):
[Unit]
Description=My Custom Script
After=network.target

[Service]
ExecStart=/usr/bin/myscript.sh
Restart=always

[Install]
WantedBy=multi-user.target


Reload systemd:
sudo systemctl daemon-reload


Enable and start the service:
sudo systemctl enable myscript
sudo systemctl start myscript



Advanced systemctl Features
Masking and Unmasking Services
Masking a service prevents it from being started, even manually.

Mask a service:
sudo systemctl mask <service-name>

Example:
sudo systemctl mask apache2


Unmask a service:
sudo systemctl unmask <service-name>



Analyzing Boot Performance
To analyze system boot time and identify slow services:
systemd-analyze

To list services by initialization time:
systemd-analyze blame

Viewing Dependencies
To view a service’s dependencies:
systemctl list-dependencies <service-name>

Example:
systemctl list-dependencies nginx

Best Practices for Using systemctl

Check Status Regularly: Use systemctl status to monitor service health.
Use Reload When Possible: Prefer reload over restart to minimize downtime.
Test Configuration Changes: After editing unit files, test with systemctl daemon-reload.
Leverage Journalctl for Logs: Combine systemctl with journalctl for detailed troubleshooting.
Avoid Masking Critical Services: Be cautious when masking essential system services.

Common systemctl Errors and Fixes

Error: “Unit not found”:

Ensure the service name is correct and the unit file exists.
Check available services with systemctl list-unit-files.


Error: “Failed to start service”:

Check logs with journalctl -u <service-name>.
Verify the service’s configuration and dependencies.


Error: “Operation not permitted”:

Ensure you’re using sudo for privileged operations.



Conclusion
systemctl is a powerful and versatile tool for managing systemd units and controlling Linux systems. By mastering its commands, you can efficiently manage services, troubleshoot issues, and optimize system performance. Whether you’re starting a web server, scheduling tasks, or analyzing boot performance, systemctl provides the flexibility and control needed for effective system administration.
For further reading, consult the official systemd documentation or use the man systemctl command to explore additional options and advanced features. With practice, systemctl will become an indispensable part of your Linux administration toolkit.
