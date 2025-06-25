## Jenkins Pipeline: Reflexion

Hier sind kurze Antworten zu den Reflexionsfragen bezüglich der lokalen Jenkins-Pipeline mit Docker:

1.  **Notwendige Schritte für Jenkins lokal mit Docker:**
    *   Docker Installation sicherstellen.
    *   Offizielles Jenkins Docker Image (`jenkins/jenkins:lts`) mit `docker run` starten.
    *   Port-Mapping (z.B. `-p 8080:8080 -p 50000:50000`) für Web-UI und Agenten.
    *   Volume-Mapping (z.B. `-v jenkins_home:/var/jenkins_home`) für Persistenz der Jenkins-Daten.
    *   Initiales Admin-Passwort aus Docker-Logs oder Datei holen und Jenkins Setup im Browser abschließen (Plugins installieren, Admin-User erstellen).
    *   Ggf. Docker-Socket ins Jenkins-Container mounten (`-v /var/run/docker.sock:/var/run/docker.sock`), um Docker-Befehle aus der Pipeline ausführen zu können (DinD - Docker in Docker).

2.  **Zweck des `Jenkinsfile` und Speicherort:**
    *   **Zweck:** Definiert die CI/CD-Pipeline als Code (Pipeline as Code). Beschreibt die Stages, Steps und Logik des Build-, Test- und Deployment-Prozesses.
    *   **Speicherort:** Muss im Wurzelverzeichnis (Root) des Git-Repositories liegen, das den Anwendungscode enthält, damit Jenkins es automatisch finden und ausführen kann, wenn ein Pipeline-Job entsprechend konfiguriert ist ("Pipeline script from SCM").

3.  **Zwei Hauptstages der Pipeline und Zweck der Steps:**
    *   **(Beispiel-Stage 1: Build & Test)**
        *   **Zweck:** Den Anwendungscode kompilieren/bauen und die Qualität durch automatisierte Tests sicherstellen.
        *   **Typische Steps:** Code auschecken, Abhängigkeiten installieren (`npm ci`), Linting, Unit-Tests ausführen (`npm test`), Anwendung bauen (`npm run build`).
    *   **(Beispiel-Stage 2: Package & Store Artifact)**
        *   **Zweck:** Das gebaute Artefakt (z.B. Frontend-Build-Ordner, Docker Image) paketieren und für spätere Deployments speichern.
        *   **Typische Steps:** Docker Image bauen (`docker build`), Image in eine Registry pushen (`docker push`), Build-Artefakte archivieren (`archiveArtifacts`).
        *(Passe die Stage-Namen und Steps an deine spezifische Pipeline an, falls abweichend.)*

4.  **Konfiguration des Git-Repositorys und Branch in Jenkins:**
    *   Beim Erstellen/Konfigurieren eines Pipeline-Jobs in Jenkins:
        *   Im Abschnitt "Pipeline" die Option "Pipeline script from SCM" auswählen.
        *   Als "SCM" "Git" auswählen.
        *   Unter "Repositories" die "Repository URL" des Git-Projekts eintragen.
        *   Optional: Credentials hinzufügen, falls das Repo privat ist.
        *   Unter "Branches to build" den gewünschten Branch angeben (z.B. `*/main`, `*/develop` oder leer für alle).

5.  **Unterschied `checkout scm` vs. `git clone`:**
    *   **`checkout scm` (Jenkins Step):**
        *   Ein von Jenkins bereitgestellter Pipeline-Step, der speziell für das Auschecken von Code aus dem konfigurierten SCM (Source Control Management, hier Git) des Jobs optimiert ist.
        *   Integriert sich nahtlos in Jenkins-Funktionen wie Changelog-Erstellung, Triggerung durch SCM-Änderungen und Workspace-Management.
        *   Verwendet die im Job-UI konfigurierten Git-Parameter (URL, Branch, Credentials).
    *   **`git clone` (manueller Befehl):**
        *   Ein Standard-Git-Befehl, der im Terminal ausgeführt wird.
        *   Erfordert die manuelle Angabe der Repository-URL und kümmert sich nicht automatisch um Jenkins-spezifische Integrationen oder Workspace-Bereinigung.
        *   In einer Pipeline müsste man ihn in einem `sh` oder `bat` Step kapseln und ggf. Credentials manuell handhaben.

---
