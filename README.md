# Static Website Migration (wird erweitert!)

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
