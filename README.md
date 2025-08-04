# Pipeline CI/CD Sécurisé pour EasyBuggy

Ce projet met en œuvre une pipeline **DevSecOps** complète avec Jenkins, intégrant l’analyse statique, la détection de vulnérabilités dans les conteneurs, l’analyse dynamique, et la sécurité des infrastructures as code (IaC). Il s'appuie sur l'application Java vulnérable [EasyBuggy](https://github.com/kazuki43zoo/buggyapp) comme démonstrateur.

---

## Technologies utilisées

- **CI/CD** : Jenkins (Pipeline déclaratif)
- **Build** : Maven 3.8.7
- **Conteneurisation** : Docker (multi-stage)
- **Analyse statique (SAST)** : SonarQube
- **Analyse de dépendances / conteneurs (SCA)** : Snyk
- **Analyse dynamique (DAST)** : OWASP ZAP
- **Sécurité IaC** : Checkov (pour les fichiers Terraform)

---

## Description des étapes Jenkins

### 1. Compilation & Analyse SonarQube
Compile le projet Java et lance une analyse de code via SonarQube.  
> Requiert un serveur SonarQube accessible à `http://localhost:9000` et un token d’authentification (`SONAR_TOKEN`).

---

### 2. Construction de l’image Docker
Utilise un Dockerfile multi-stage pour builder et packager l’application Java.  
> Image générée : `asecurityguru/testeb`

---

### 3. Scan de conteneur avec Snyk
Lance une analyse de vulnérabilités sur l’image Docker avec [Snyk CLI](https://snyk.io/).

---

### 4. Analyse de dépendances Maven avec Snyk
Vérifie les bibliothèques du projet Java (via `pom.xml`) pour détecter les vulnérabilités connues.

---

### 5. Analyse dynamique avec OWASP ZAP
Effectue un scan DAST sur l’URL cible `https://www.example.com`.  
> Rapport HTML généré : `C:\SAST\ZAP_2.16.0\Output.html`

---

### 6. Scan IaC avec Checkov
Analyse le fichier Terraform `main.tf` pour identifier des erreurs de configuration ou failles de sécurité.

---

## Dockerfile – Build multi-stage

- Étape 1 : Build Maven
- Étape 2 : Image légère avec OpenJDK 8 + exécution de `easybuggy.jar`
- Nombreux paramètres JVM configurés pour le monitoring, la mémoire, le debug, les logs GC...

---

## Credentials nécessaires dans Jenkins

| ID du secret     | Utilisation                          |
|------------------|---------------------------------------|
| `SONAR_TOKEN`    | Authentification SonarQube            |
| `SNYK_TOKEN`     | Authentification Snyk                 |
| `dockerlogin`    | Connexion au registre DockerHub       |

---

## Pré-requis

- Jenkins installé (avec support Pipeline, Docker)
- Docker installé
- SonarQube en local ou distant
- Snyk CLI disponible (`snyk-win.exe` ou `snyk` via PATH)
- OWASP ZAP installé (`zap.sh`)
- Checkov installé (`checkov` via PATH)

---

Auteur

Badaoudou Barro

---
