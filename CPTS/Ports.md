
| Port(s)         | Protocol              |
| --------------- | --------------------- |
| `20`/`21` (TCP) | `FTP`                 |
| `22` (TCP)      | `SSH`                 |
| `23` (TCP)      | `Telnet`              |
| `25` (TCP)      | `SMTP`                |
| `80` (TCP)      | `HTTP`                |
| `161` (TCP/UDP) | `SNMP`                |
| `389` (TCP/UDP) | `LDAP`                |
| `443` (TCP)     | `SSL`/`TLS` (`HTTPS`) |
| `445` (TCP)     | `SMB`                 |
| `3389` (TCP)    | `RDP`                 |
- Web servers usually run on TCP ports `80` or `443`
- Priviligierte ports `1 to 1023`
- Ports go from `1 to 65,535`
- Port `3389` ist default Port für `Remote Desktop Services` -- Windows
- Port `22` -- Linux (Kann aber auch Windows sein)


# Netcat

Banner Grabbing - Siehe welcher Service auf dem Port läuft
```bash
netcat ip port
```


# Nmap

| Flag              | Descripton                                                     |
| ----------------- | -------------------------------------------------------------- |
| `-sV`             | Version Scan - service protocol, application name, and version |
| `-sC`             | Use Nmap Scripts                                               |
| `-p-`             | Scan all ports                                                 |
| `--script skript` | Kannst ein bestimmtes Skript angeben                           |
| `-o`              | Betriebssystem Erkennung                                       |
| `-A`              | Aggressive Scan                                                |

Basic Scan - only scan the 1023 well known ports
```bash
nmap ip
```

| State      | Descripton                                        |
| ---------- | ------------------------------------------------- |
| `open`     | Open                                              |
| `filtered` | Firewall erlaubt nur Zugriff von spezifischen IPs |
| ``         |                                                   |


Banner Grabbing
```bash
nmap -sV --script=banner target
```


# FTP

Verbinde dich zu ftp server
```bash
ftp -p ip
```


# SMB

- On Windows Machine
- Vector for vertical and leteral movement
- EthernalBlue
- Nmap has scripts for enumarating SMB like `smb-os-discovery.nse`

| smbclient | Descripton                        |
| --------- | --------------------------------- |
| `-N`      | Unterdrückt die Passwort Freigabe |
| `-L`      | Gib Share Liste                   |
| `get`     |                                   |
|           |                                   |

Enumerate SMB OS
```bash
nmap --script smb-os-discovery.nse -p445 ip
```

 
Zeige alle Shares an
```bash
smbclient -N -L \\\\ip
```

Get a Share
```bash
smbclient \\\\ip\\user
```

Get a Share with credentials (passwort wird dann gefragt)
```bash
smbclient -U bob \\\\ip\\users
```

	Wenn du verbunden bist, kannst du mit get Dateinamen die Datei runterladen



# SNMP

- Version `1 and 2c` only need a plaintext username to access. `Version 3` needs passwort and has encryption#

Example Service Scanning
```shell-session
snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0
```

| snmpwalk            | Descripton                                                                        |
| ------------------- | --------------------------------------------------------------------------------- |
| `-v`                | Versiom                                                                           |
| `-c`                | public or private acts like a password                                            |
| `1.3.6.1.2.1.1.5.0` | Ist ein OID (Object Identifier), dieser dient als sysName, also Hostname des Ziel |
|                     |                                                                                   |
You can use `onesixtyone` to bruteforce community string names using a file containing strings
```shell
onesixtyone -c dict.txt 10.129.42.254
```
