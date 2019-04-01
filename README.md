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

**Ein großer Dank geht an funkypenguin (geek-cookbook.funkypenguin.co.nz) & gkoerk (https://blog.qnapunofficial.com/docker-on-qnap & discord https://discordapp.gg/Zj9EYsf)**
