# Static Website Migration

Dieses Projekt demonstriert, wie eine statische Website mithilfe von Docker containerisiert und anschließend auf einem Cloud-Server (Hetzner) bereitgestellt wird.
Der Fokus liegt darauf, eine reproduzierbare, serverunabhängige Deployment-Umgebung zu schaffen, ein wichtiger Bestandteil moderner DevOps-Workflows.

### Projektziel

Dieses Projekt zeigt, wie eine einfache statische Website mit Docker containerisiert und anschließend auf einem Cloud-Server betrieben wird.

Dabei dient Docker als reproduzierbare Laufzeitumgebung, sodass das Deployment unabhängig von Serverkonfigurationen zuverlässig funktioniert.

### Features

- Docker-Container mit Nginx als Webserver  
- Reproduzierbare Build-Umgebung durch Dockerfile 
- Deployment auf einem Cloud-Server (Hetzner)  
- Zero-Config: keine manuellen Konfigurationen auf dem Server 
- Statische HTML-Website inklusive
- Infrastruktur leicht portierbar (lokal, Cloud, CI/CD)




## Architektur-Diagramm
```
┌──────────────────────────┐
│   GitHub Repository      │
│  (Website + Dockerfile)  │
└──────────────┬───────────┘
               │ git clone
               ▼
┌──────────────────────────┐
│   Cloud Server (Hetzner) │
│ Ubuntu + Docker Engine   │
└──────────────┬───────────┘
               │ docker build
               ▼
┌──────────────────────────┐
│  Docker Image (Nginx)    │
│  enthält index.html      │
└──────────────┬───────────┘
               │ docker run
               ▼
┌──────────────────────────┐
│  Container läuft auf 80  │
│ Website öffentlich über: │
│ http://SERVER-IP         │
└──────────────────────────┘
```

**Schritte zum Deployment**

**1.**  Repository klonen

```bash
git clone https://github.com/dein-user/static-website-migration.git
cd static-website-migration
```


**2.**  Docker Image bauen

```bash
docker build -t buergerportal .

```


**3.**  Container starten

```bash
docker run -d -p 80:80 buergerportal
```


Die Website ist jetzt erreichbar unter:

```bash
http://localhost
```

oder auf dem Server:
```bash
http://<SERVER-IP>
```

## Deployment auf einem Cloud-Server (Hetzner)

Dieses Projekt wird nicht nur lokal ausgeführt, sondern auch auf einem Cloud-Server betrieben.  
Dazu wurde eine virtuelle Maschine (VM) bei Hetzner erstellt und manuell konfiguriert.


**1. Server erstellen (Hetzner Cloud)**

**1.** Login in die Hetzner Cloud Console  
**2.** Neues Projekt anlegen  
**3.** Neue VM erstellen:
   
   - **Typ:** CPX22  
   - **Image:** Ubuntu 24.04  
   - **Region:** Falkenstein (Germany)  

**4.** SSH root-Zugang erhalten oder Passwort setzen 
Die Maschine war anschließend unter einer öffentlichen IP erreichbar:
```Powershell
http://<SERVER-IP>
```
**2. Verbindung zum Server herstellen**

Auf deinem lokalen Rechner (Windows PowerShell):
```bash
ssh root@<SERVER-IP>
```

Beim ersten Login fordert der Server eine Passwortänderung.

**3. Docker auf dem Server installieren**

Auf dem Server:
```bash
apt update
apt install -y ca-certificates curl gnupg
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
chmod a+r /etc/apt/keyrings/docker.asc
```

Repository hinzufügen:
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" \
  > /etc/apt/sources.list.d/docker.list
```

Docker Engine installieren:
```bash
apt update
apt install -y docker-ce docker-ce-cli containerd.io
```

Test:
```bash
docker --version
```
**4. Projekt auf den Server kopieren**

Auf lokaler PowerShell:
```Powershell
scp Dockerfile index.html root@<SERVER-IP>:/opt/static-website-migration/
```

Dann auf dem Server:
```bash
cd /opt/static-website-migration
```
**5. Docker Image auf dem Server bauen**

Auf dem Server:
```bash
docker build -t buergerportal .
```
**6. Container starten**
```bash
docker run -d -p 80:80 --name buergerportal-container buergerportal
```
Der Container läuft jetzt als Webserver auf Port 80.

**7. Website aufrufen**

Von jedem Computer:
```bash
http://<SERVER-IP>
```

Beispiel:
```
http://203.0.113.42
```
Damit wird die statische HTML-Seite ausgeliefert.

### Dateien im Projekt
Das Dockerfile verwendet ein minimales Nginx-Basisimage und kopiert die statische index.html in das Standard-Webverzeichnis /usr/share/nginx/html.

| Datei        | Beschreibung                                  |
|--------------|-----------------------------------------------|
| `Dockerfile` | Definiert, wie das Nginx-Image gebaut wird    |
| `index.html` | Statische HTML-Website                         |
| `README.md`  | Projektdokumentation                           |


### Warum dieses Projekt?

Es zeigt grundlegende Kenntnisse in:

- Wie Docker Images gebaut werden
- Wie Container isolierte Webserver bereitstellen
- Wie man einen Linux-Cloud-Server (Hetzner) einrichtet
- Wie Deployment ohne manuelle Konfiguration funktioniert
- Grundlagen von Port-Forwarding, SSH, Firewall, Webserver



### Projektstruktur
static-website-migration
```/
│
├── Dockerfile
├── index.html
└── README.md
```
