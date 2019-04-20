**Information for using this Repo**

for using this Repo you don't need much to do

1. create an Overlay network
   `docker network create --driver=overlay --subnet=172.1.1.0/22 --attachable traefik_public`

2. create a Folder structur
   - main Folder
     - config
     - extra folder's as you can read in \*.yml
     - runtime
       - subfolders (as you can see in \*.yml)
3. edit \*.yml
   - /share/appdata configure to own Folderstructur
   - example.com adjust to your own domain
   - not needed components comment out/outcommented needed components reverse
   - adjust the provider if you don't use Github as oAuth (old system)
4. edit \*.env
   - adjust oAuth (old system)
   - PUID/GUID adjust to own needs
5. edit Traefik
   - in the example it use DNS via Cloudflare pls look into the documetary from Traefik

---

## Preparation on QNAP NAS

1. bring Qnapclub as Repository in QNAPStore (https://www.qnapclub.eu/en/howto/1)
2. install Entware-std from the new Repository
3. install Container Station
4. make Ports 80/443/8080 free (deactivate Web and Switch Dashbaord to another Port for example 4000)
5. open the Container Station and switch to preferences
   -Networksettings (docker0) change IP 10.0.4.x/22
   - primary DNS put in cloudflare or google
   - admit
6. connect via SSH and put this in "docker swarm init"
   - should the be an ERROR then use this flag "--advertise-addr 192.168.x.x" with docker swarm init
7. in /opt/etc/.profile insert this

```
dsd(){
 docker stack deploy "$1" -c /share/appdata/config/"$1"/"$1".yml
}
dsr(){
 docker stack rm "$1"
}
bounce(){
 docker stack rm "$1"
 sleep 15
 docker stack deploy "$1" -c /share/appdata/config/"$1"/"$1".yml
}
```

7. set the permissions for necessary Folders
8. acme.json must be empty and permissions set to 600
9. Setup Auth0 after that u can start with the command "dsd traefik"
   - with DNS challenging it's a good idea to do Testruns
   - this 3 lines reverse the comment then do "dsd traefik" wait and look if acme.json get some data (when Dashboard is loaded it needs around 5 min to get the test cert shown), if everything works "dsr traefik" comment the 3 lines out and "dsd traefik"
   - Traefik.toml

```
#acmeLogging = true
#onDemand = true
#caServer = "https://acme-staging-v02.api.letsencrypt.org/directory"
```

10. start with the other apps

## setup Auth0

for Using this forwardauth setup you have to register on Auth0

1. register on Auth0
2. make a new App
   - Application Type: Regular Web Application
   - Allowed Callback Urls: https://auth.example.com/signin
   - Allowed Web Origins: https://auth.example.com
3. Connections -> Social you can use up to 2 sociallogins (google,github as an example)
   - You get for every Loginhelper a help page (How to Obtain a Client ID don't forget to check email you need that later)
   - Try if the Setup work - so you don't need to look later for troubles
4. build a Rule - Whitelist. You only need to change the mail.
5. Auth0 Client ID and Client Secret put in the application.yaml

**Improvments are Welcome, Just let me know**

It's not bad to ask for help. Sharing is Caring!

## **many thx to funkypenguin (geek-cookbook.funkypenguin.co.nz) & gkoerk (https://blog.qnapunofficial.com/docker-on-qnap & discord https://discordapp.gg/Zj9EYsf)**

**Informationen zum Benutzen dieses Repos**

Um dieses Repo zu benutzen muss nicht viel gemacht werden

1. Overlay Netzwerk erstellen erstellen
   `docker network create --driver=overlay --subnet=172.1.1.0/22 --attachable traefik_public`

2. Ordner Struktur anlegen
   - überordner
     - Config
     - Zusätzliche Ordner wie in den \*.yml ersichtlich
     - runtime
       - Unterordner
3. \*.yml anpassen
   - /share/appdata anpassen an die eigene Ordnerstruktur (Volumes)
   - example.com anpassen an die eigene Domain
   - nicht benötigte Teile auskommentieren/kommentieren rückgängig machen
   - Provider anpassen wenn anderer oAuth Server benutzt werden soll als Github (altes System)
4. \*.env anpassen
   - oAuth Daten anpassen (altes System)
   - PUID/GUID an eigene bedürfnisse anpassen
5. Traefik anpassen
   - in diesem Bsp wurde DNS via Cloudflare benutzt hier bitte die Dokumentation von Traefik beachten

---

## Vorbereitungen in QNAP treffen

1. Qnapclub als Repo im QNAPStore hinzufügen (https://www.qnapclub.eu/de/howto/1)
2. Im neuen Repo Entware-std suchen & installieren
3. Container Station installieren
4. Die Ports 80/443/8080 freimachen (Web deaktivieren und Dashboard auf einen anderen Port legen zB. 4000)
5. Öffne die Container Station gehe zu den präferenzen wechseln den tab oben auf
   - Netzwerkeinstellungen (docker0) IP-Adresse auf 10.0.4.x/22 ändern
   - Primär DNS-Server entweder google oder cloudflare eintragen
   - das ganze übernehmen
6. via SSH verbinden und "docker swarm init" eingeben
   - sollte ein fehler kommen den flag "--advertise-addr 192.168.x.x" bei docker swarm init benutzen
7. in /opt/etc/.profile folgendes hinzufügen

```
dsd(){
 docker stack deploy "$1" -c /share/appdata/config/"$1"/"$1".yml
}
dsr(){
 docker stack rm "$1"
}
bounce(){
 docker stack rm "$1"
 sleep 15
 docker stack deploy "$1" -c /share/appdata/config/"$1"/"$1".yml
}
```

7. bei allen ordnern die permissions setzen
8. acme.json muss leer sein und die permissions auf 600 gesetzt sein
9. Auth0 einrichten danach kann man mit "dsd traefik" starten
   - bei DNS Challenge empfiehlt es sich testläufe zu machen
   - diese 3 reinkommentieren dsd traefik warten und überprüfen ob die acme.json befüllt wird(wenn das Dashboard geladen ist sollte nach ca. 5 min ein TestZertifikat angezeigt werden), wenn ja "dsr traefik" auskommentieren wieder und dsd traefik ausführen
   - Traefik.toml

```
#acmeLogging = true
#onDemand = true
#caServer = "https://acme-staging-v02.api.letsencrypt.org/directory"
```

10. mit den anderen Apps fortfahren

---

## Auth0 einrichten

Um ForwardAuth zu benutzen muss man sich auf Auth0 anmelden.

1. Anmeldung bei Auth0
2. Eine neue Applikation erstellen
   - Application Type: Regular Web Application
   - Allowed Callback Urls: https://auth.example.com/signin
   - Allowed Web Origins: https://auth.example.com
3. Connections -> Social bis zu 2 "Loginhilfen" einrichten
   - Es gibt bei jedem ensprechenden Eintrag eine Hilfe (How to Obtain a Client ID nicht vergessen e-mail anzuhacken wo es möglich ist wird später benötigt)
   - Auf Try tippen bei den eingerichteten Accounts um später fehler auszuschliessen
4. Bei Rules eine Whitelist erstellen. Man benötigt nur die mail adressen zu ändern.
5. Auth0 Client ID und Client Secret in die application.yaml eintragen

---

**Für verbesserungen bin ich gerne zu haben. Einfach bescheid geben**

Es ist nicht schlimm um hilfe zu bitten. Sharing is Caring!

**Ein großer Dank geht an funkypenguin (geek-cookbook.funkypenguin.co.nz) & gkoerk (https://blog.qnapunofficial.com/docker-on-qnap & discord https://discordapp.gg/Zj9EYsf)**
