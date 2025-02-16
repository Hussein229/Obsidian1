Use Checklists from [HackTricks](https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html) or [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)

## Enumeration Scripts
Linux enumeration scripts include [LinEnum](https://github.com/rebootuser/LinEnum.git) and [linuxprivchecker](https://github.com/sleventyeleven/linuxprivchecker), and for Windows include [Seatbelt](https://github.com/GhostPack/Seatbelt) and [JAWS](https://github.com/411Hall/JAWS).

Another useful tool we may use for server enumeration is the [Privilege Escalation Awesome Scripts SUITE (PEASS)](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite), as it is well maintained to remain up to date and includes scripts for enumerating both Linux and Windows.

To use PEASS automated enumeration script:
```shell-session
./linpeas.sh
```

#### Check what sudo priviledges you have
```
sudo -l
```
```
sudo su
```

Mach einen `sudo -l`, dann siehst du welche rechte du hast mit einem anderen account. Um dann mit dem anderen user den Befehl der da steht ausführen zu können mach folgendes:
Um mit den Nutzer der die sudo berechtigung hat einen befehl durchzuführen:
```shell-session
sudo -u user /bin/echo Hello World!
```

 - [GTFOBins](https://gtfobins.github.io/) contains a list of commands and how they can be exploited through `sudo`
 - [LOLBAS](https://lolbas-project.github.io/#) LOLBAS (Living Off The Land Binaries and Scripts) ist ein Projekt, das eine Liste von legitimen Windows-Systemdateien (wie EXE-, DLL- und Script-Dateien) sammelt, die für sicherheitskritische Funktionen missbraucht werden können. Diese Dateien sind standardmäßig in Windows enthalten und werden von Administratoren und dem Betriebssystem selbst genutzt.

In both Linux and Windows, there are methods to have scripts run at specific intervals to carry out a task. Some examples are having an anti-virus scan running every hour or a backup script that runs every 30 minutes. There are usually two ways to take advantage of scheduled tasks (Windows) or cron jobs (Linux) to escalate our privileges:

1. Add new scheduled tasks/cron jobs
2. Trick them to execute a malicious software

The easiest way is to check if we are allowed to add new scheduled tasks. In Linux, a common form of maintaining scheduled tasks is through `Cron Jobs`. There are specific directories that we may be able to utilize to add new cron jobs if we have the `write` permissions over them. These include:

1. `/etc/crontab`
2. `/etc/cron.d`
3. `/var/spool/cron/crontabs/root`

If we can write to a directory called by a cron job, we can write a bash script with a reverse shell command, which should send us a reverse shell when executed.

____________
#### SSH Keys

Falls man **read access** over the `.ssh` directory 
- Dann kannst du die private ssh keys found in `/home/user/.ssh/id_rsa` or `/root/.ssh/id_rsa` lesen
	- Wenn wir den key zb aus `/root/.ssh/id_rsa` auslesen können, können wir uns anmelden bei dem User, *kopiere* den key, pack ihn auf dein home verzeichnis und dann chmod darauf und dann ssh
```
	vim id_rsa
	chmod 600 id_rsa
	ssh root@ip -i id_rsa
```

Falls man **write access** hat  to a users`/.ssh/` directory, dann kannst du deinen public key in den user ssh `directory /home/user/.ssh/authorized_keys` tun.
 - Create a key
 ```shell-session
ssh-keygen -f key
```
Output sind 2 keys -- private und pub. Den Pub key fügen wir hier `/root/.ssh/authorized_keys` hinzu:
```shell-session
echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys
```
Now, the remote server should allow us to log in as that user by using our private key:
```shell-session
ssh root@10.10.10.10 -i key
```
