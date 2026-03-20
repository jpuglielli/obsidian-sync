## Daemon
- Just a process that runs in the background continuously
- The "d" suffix = daemon (systemd is a daemon process)
## Systemd
**Vocab:**
- **Units** are just systemd's word for "a thing systemd manages." A unit is defined by a config file. There are several _types_ of units, but the one you care about 95% of the time is a **service**.
- **Service**: specific type of unit that represent a running process — nginx, postgres, sshd, kubelet, etc. 
	- When people say "systemd service" they mean a long-running daemon that systemd starts, monitors, and restarts if it dies.
**General notes:**
- First process the Kernel launches on startup
	- Computer, linux VM, etc...
	- PID 1
- Starts all other **services**
	- A **service** is just a name for a running process, which runs in the background as a daemon.
	- Web server
	- Database
	- Networking
	- SSH daemon
	- etc...
- Once services are running, restarts them if they crash, shuts them down cleanly, manages dependencies between them
**Reading service status:**
- `systemctl status nginx` — is it running, when did it start, recent logs
- `systemctl list-units --failed` — what's broken on this machine
**Controlling services:**
- `systemctl start/stop/restart nginx`
- `systemctl enable nginx` — start automatically on boot
- `systemctl disable nginx` — don't start on boot
**Reading logs (journalctl):**
Systemd's log viewer, logs for the whole machine. Can filter
- `journalctl -u nginx` — logs for a specific service
- `journalctl -u nginx --since "5 min ago"` — recent logs
- `journalctl -f` — tail all system logs live
## Signals
- Messages sent by the kernel (or other processes) to a running process to notify it of an event or tell it to do something
- Primary way for OS to communicate with processes
- Process can either handle a signal, or ignore it (besides SIGKILL)
![[Pasted image 20260318193111.png]]
**The graceful shutdown pattern (used by systemd, Kubernetes, Docker):**
1. Send SIGTERM → process finishes in-flight work, cleans up, exits
2. Wait grace period (K8s: 30s, systemd: 90s)
3. If still alive → send SIGKILL, process is terminated immediately, no cleanup
## `ps`
"**P**rocess **S**napshot": What's running right now
-  `ps aux` — show all processes on the machine (user, PID, CPU%, memory%, command)
- `ps aux | grep nginx` — "is nginx running? what's its PID?"
## `top` / `htop`
"table of processes": Live process monitor. Same as ps but real-time and continuously updating. Shows processes sorted by resource usage.
- Use these to answer "what is eating all the CPU/memory right now?" — the first thing you'd run when a machine is slow or unresponsive.
## Background Processes
A **background process** is just a process that runs without occupying your terminal, so you can keep typing other commands.
- Append `&` to run something in the background: `./long-script.sh &`
-  `Ctrl+Z` suspends a running process, then `bg` resumes it in the background
- `fg` brings a background process back to the foreground
- `jobs` lists your background processes