**Informationen zum Benutzen dieses Repos**

Um dieses Repo zu benutzen muss nicht viel gemacht werden

1. Overlay Netzwerk erstellen erstellen
   docker network create --driver=overlay --subnet=172.1.1.0/22 --attachable traefik_public

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
   - Provider anpassen wenn anderer oAuth Server benutzt werden soll als Github
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
4. Die Ports 80/443/8080 freimachen (Web deaktivieren und Dashboard auf einen anderen Port legen)
4. Öffne die Container Station gehe zu den präferenzen wechseln den tab oben auf 
   - Netzwerkeinstellungen (docker0) IP-Adresse auf 10.0.4.x/22 ändern
   - Primär DNS-Server entweder google oder cloudflare eintragen
   - das ganze übernehmen
5. via SSH verbinden und "docker swarm init" eingeben
   - sollte ein fehler kommen den flag "--advertise-addr 192.168.x.x" bei docker swarm init benutzen
6. in /opt/etc/.profile folgendes hinzufügen
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
