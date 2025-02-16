Tools to use for Web Enumeration (reveal hidden files or directories, that shouldnt be accessible online):
- ffuf - good for fuzzing
- GoBuster

| Code  | Description |
| ----- | ----------- |
| `200` | `Success`   |
| `403` | `Forbidden` |
| `301` | `Redirect`  |
| ``    | ``          |
Reverse-DNS-Lookup um von einer IP die DNS Adresse zu bekommen - klappt nur mit IP nicht mit port
```bash
host 94.237.59.180
```
oder
```
nslookup 94.237.59.180
```

#### GoBuster

Perform:
- DNS, vhost and directory/file brute-force
- Enumeration of public AWS S3 buckets

Directory(and file) Brute force
```bash
gobuster dir -u http://ip/ -w pfadZurWordlist
```

==Subdomain Enumeration==
Use [seclist](https://github.com/danielmiessler/SecLists) it provides good txt files
```shell-session
git clone https://github.com/danielmiessler/SecLists
sudo apt install seclists -y
```

```shell
gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt
```
![](CPTS/img/105f989aa9917b1b41b5d07668aa88ef_MD5.jpeg)

Subdomin Enumeration auf eine IP mit Port
```bash
gobuster vhost -u http://94.237.59.180:48032 -w /usr/share/SecLists/Discovery/DNS/namelist.txt

```
________________________________________________________________________

#### Banner grapping using curl. 
`-I → Fordert nur die **HTTP-Header** an (ohne den Body der Seite zu laden). `
**`-L`** → Folgt automatisch Weiterleitungen (HTTP 3xx-Statuscodes).
```shell
curl -IL https://www.inlanefreight.com
```

_____________________________________________________________________________________

#### whatweb
```shell
whatweb 10.10.10.121

http://10.10.10.121 [200 OK] Apache[2.4.41], Country[RESERVED][ZZ], Email[license@php.net], HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[10.10.10.121], Title[PHP 7.4.3 - phpinfo()]
```

```shell-session
whatweb --no-errors 10.10.10.0/24

http://10.10.10.11 [200 OK] Country[RESERVED][ZZ], HTTPServer[nginx/1.14.1], IP[10.10.10.11], PoweredBy[Red,nginx], Title[Test Page for the Nginx HTTP Server on Red Hat Enterprise Linux], nginx[1.14.1]
http://10.10.10.100 [200 OK] Apache[2.4.41], Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[10.10.10.100], Title[File Sharing Service]
http://10.10.10.121 [200 OK] Apache[2.4.41], Country[RESERVED][ZZ], Email[license@php.net], HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[10.10.10.121], Title[PHP 7.4.3 - phpinfo()]
http://10.10.10.247 [200 OK] Bootstrap, Country[RESERVED][ZZ], Email[contact@cross-fit.htb], Frame, HTML5, HTTPServer[OpenBSD httpd], IP[10.10.10.247], JQuery[3.3.1], PHP[7.4.12], Script, Title[Fine Wines], X-Powered-By[PHP/7.4.12], X-UA-Compatible[ie=edge]
```

______________________

#### Certificates
View them, they could cointain interesting Information

___

#### Robots.txt
In dieser Datei stehen die Seiten bzw Verzeichnisse die angezeigt bzw nicht angezeigt werden dürfen.  bsp:
```
User-agent: *
Disallow: /admin/
Disallow: /private/
Allow: /
```

|Code|Description|
|---|---|
|`User-agent: *`|`Gilt für alle Suchmaschinen-Bots`|
|`User-agent: Googlebot`|`Spezifische Regeln nur für den Googlebot`|
|`Disallow: /`|`Verbietet das Indexieren der gesamten Website`|
|`Disallow: /secret/`|`Verbietet das Crawlen des /secret/-Verzeichnisses`|
|`Allow: /public/`|`Erlaubt das Crawlen des /public/-Verzeichnisses`|
|`Sitemap: https://example.com/sitemap.xml`|`Gibt die Sitemap-URL an`|

___

#### Source Code
Hit `[CTRL + U]` um anzuzeigen. Da können manchmal interessante Sachen drinnen stehen.


