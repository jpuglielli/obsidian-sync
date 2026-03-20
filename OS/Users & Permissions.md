## `chmod`
"**ch**ange **mod**e" — as in, change the file's mode bits
- `chmod` changes file **permissions** 
	- who can **read**, **write**, and **execute** a file. 
- Linux permissions are divided across three [[#Entities]]
## `chown`
**ch**ange **own**er
- Changes the owner and/or group of a file. 
- Only root can change the owner
## Entities
1. **Owner (u)**: the single user who owns the file. 
	- By default, that's whoever created it. 
	- You can change ownership with `chown`. 
	- The owner is the only non-root user who can `chmod` the file, regardless of what permissions are set.
2. **Group (g)**: If your user is in the file's group, you're evaluated through the group permission bits.
	- Every file belongs to exactly one group. 
	- By default, it's the primary group of the user who created it. 
	- Changed with `chgrp`or `chown user:group`. 
	- Users can be members of many groups, but the file only checks against this one group. 
3. **Others (o)**: Everyone else — any authenticated user on the system who is neither the owner nor a member of the file's group. 
	- This is the catch-all. 
	- It's the most restrictive set of permissions on most files, because you're granting access to anyone with a login.
## `sudo`
**s**ubstitute **u**ser, **do**
- sudo is a program which lets permitted users run commands as **another user**
- Default is as [[#`root`]]
- The config file `/etc/sudoers` controls who can sudo into what
## `root`
Identity which bypasses all permissions checks
- UID 0
- *(Note there are many kinds of ids, UID, PID, GID. Link later)*
## Why do we never run [[Docker|containers]] as root?
- Containers are not separate machines, they are regular Linux processes isolated by two [[Kernel|kernel]] features:
    - **Namespaces**: limit what a process can _see_ (its own PID tree, filesystem mounts, network stack)
    - **Cgroups**: limit what a process can _use_ (CPU, memory, I/O)
- Both are enforced by the shared host kernel, there is no hardware boundary like a VM
- UID 0 (root) inside the container **is** UID 0 on the host kernel
- If a root container process escapes its namespace (via kernel bug or misconfiguration), it has full root access to the host, all files, all other containers, kubelet, secrets
**Mitigations:**
- `USER` directive in Dockerfile to run as non-root UID
- `runAsNonRoot: true` in K8s security context
- User namespace remapping (maps container UID 0 → unprivileged host UID)