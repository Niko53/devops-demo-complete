# Pacchetto completo demo DevOps

Questa cartella contiene una piccola applicazione web statica pensata per esercitazioni e lezioni DevOps. Include frontend HTML, configurazione Nginx, Dockerfile, `docker-compose.yml`, manifest Kubernetes e una pipeline GitHub Actions di base. [cite:1]

## Contenuto del pacchetto

```text
devops-demo-complete/
├── devops-demo-app.html
├── Dockerfile
├── nginx.conf
├── docker-compose.yml
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
└── .github/
    └── workflows/
        └── deploy.yml
```

## Obiettivo didattico

Questa demo è utile per mostrare un flusso DevOps completo ma semplice: creazione del repository, versionamento con Git, esecuzione locale, containerizzazione con Docker, avvio con Docker Compose, e base di deploy su Kubernetes. [cite:1]

## Prerequisiti

Prima di iniziare, verifica di avere installato:

- Git
- Docker Desktop oppure Docker Engine + Docker Compose plugin
- Un editor come VS Code
- Facoltativo: kubectl e un cluster locale come Docker Desktop Kubernetes, kind o minikube, se vuoi arrivare anche alla parte Kubernetes [cite:1]

## Procedura passo passo

### 1. Salvataggio iniziale in Git

1. Crea una cartella di lavoro sul tuo computer, ad esempio `devops-demo-complete`.
2. Copia dentro tutti i file del pacchetto.
3. Apri il terminale nella cartella del progetto.
4. Inizializza Git:

```bash
git init
```

5. Imposta il branch principale:

```bash
git branch -M main
```

6. Aggiungi tutti i file al repository:

```bash
git add .
```

7. Esegui il primo commit:

```bash
git commit -m "Initial commit - devops demo app"
```

A questo punto il progetto è salvato localmente in Git. [cite:1]

### 2. Pubblicazione su GitHub

1. Crea un nuovo repository vuoto su GitHub, ad esempio `devops-demo`.
2. Collega il repository locale al repository remoto sostituendo `YOUR-USER` con il tuo username GitHub:

```bash
git remote add origin https://github.com/YOUR-USER/devops-demo.git
```

3. Invia il branch `main` su GitHub:

```bash
git push -u origin main
```

Da questo momento il progetto è versionato sia in locale sia su GitHub. [cite:1]

### 3. Richiamo del progetto da Git in locale

Se vuoi recuperare il progetto su un altro computer o simulare il flusso completo, usa:

```bash
git clone https://github.com/YOUR-USER/devops-demo.git
cd devops-demo
```

Questo passaggio è utile per spiegare il concetto di repository remoto, clonazione e bootstrap dell'ambiente locale. [cite:1]

### 4. Esecuzione locale senza Docker

Essendo una pagina statica, puoi provarla anche in locale in modo semplice.

#### Opzione A: con Python

```bash
python3 -m http.server 8080
```

Poi apri il browser su:

```text
http://localhost:8080/devops-demo-app.html
```

#### Opzione B: con VS Code Live Server

- Apri la cartella in VS Code.
- Avvia l'estensione Live Server sul file `devops-demo-app.html`.
- Verifica che l'app si apra correttamente nel browser.

Questa fase è utile per mostrare la differenza tra esecuzione locale semplice e deploy containerizzato. [cite:1]

### 5. Dockerizzazione locale

Per costruire l'immagine Docker, esegui:

```bash
docker build -t devops-demo:local .
```

Per avviare il container:

```bash
docker run --name devops-demo -p 8080:80 devops-demo:local
```

Apri poi il browser su:

```text
http://localhost:8080
```

Per fermare il container:

```bash
docker stop devops-demo
```

Per rimuoverlo:

```bash
docker rm devops-demo
```

Questa fase permette di spiegare build context, immagine, container, port mapping e differenza tra artefatto e runtime. [cite:1]

### 6. Funzionamento in Docker locale con Docker Compose

Per avviare la demo usando `docker-compose.yml` o il plugin Compose integrato:

```bash
docker compose up --build
```

L'app sarà disponibile su:

```text
http://localhost:8080
```

Per fermarla:

```bash
docker compose down
```

Questo passaggio è utile per introdurre l'idea di orchestration locale, utile prima di passare a Kubernetes. [cite:1]

### 7. Modifica del codice e nuovo salvataggio in Git

Per mostrare un tipico ciclo DevOps, puoi cambiare un testo della homepage, ad esempio la versione mostrata nella UI o il titolo principale. Poi esegui:

```bash
git status
git add .
git commit -m "Update homepage copy"
git push
```

Questa sequenza è utile per collegare modifica del codice, commit, push e successiva pipeline CI/CD. [cite:1]

### 8. Funzionamento della pipeline GitHub Actions

Il file `.github/workflows/deploy.yml` definisce un workflow che:

- esegue il checkout del repository,
- configura Docker Buildx,
- effettua il login su GitHub Container Registry,
- costruisce l'immagine,
- pubblica l'immagine su GHCR. [cite:1]

Per usarlo davvero, devi sostituire nel progetto i riferimenti generici con i tuoi valori GitHub, in particolare owner e nome immagine. [cite:1]

### 9. Base per deploy Kubernetes

I file in `k8s/` forniscono una base semplice per una spiegazione in aula:

- `deployment.yaml` crea 2 repliche del container con probe su `/ready` e `/health`. [cite:1]
- `service.yaml` espone il servizio all'interno del cluster come `ClusterIP`. [cite:1]

Per applicarli in un cluster locale:

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

Per verificarne lo stato:

```bash
kubectl get pods
kubectl get svc
kubectl describe deployment devops-demo
```

## Sequenza consigliata per la lezione

Un ordine didattico semplice e molto efficace può essere questo:

1. Mostrare i file del progetto.
2. Inizializzare Git e fare il primo commit.
3. Pubblicare su GitHub.
4. Clonare il repository su un altro path o un altro computer.
5. Eseguire la pagina in locale senza container.
6. Costruire l'immagine Docker.
7. Avviare il container in locale.
8. Usare Docker Compose.
9. Fare una piccola modifica e un nuovo commit.
10. Mostrare la pipeline GitHub Actions.
11. Chiudere con i manifest Kubernetes. [cite:1]

## Comandi essenziali raccolti

```bash
# init repo
git init
git branch -M main
git add .
git commit -m "Initial commit - devops demo app"

# collega GitHub
git remote add origin https://github.com/YOUR-USER/devops-demo.git
git push -u origin main

# clone da GitHub
git clone https://github.com/YOUR-USER/devops-demo.git
cd devops-demo

# esecuzione locale semplice
python3 -m http.server 8080

# docker build + run
docker build -t devops-demo:local .
docker run --name devops-demo -p 8080:80 devops-demo:local

# compose
docker compose up --build
docker compose down
```

## Nota pratica

Per una demo in aula, questa struttura è ideale perché rimane abbastanza piccola da essere compresa rapidamente, ma abbastanza realistica da permettere di introdurre repository, immagini container, probe, registry e deploy progressivi. [cite:1]
