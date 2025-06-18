# Tutoriel : lancer EmpireTown sur une machine virtuelle Windows

Ce guide explique comment mettre en route le serveur EmpireTown (projet LunarisV4) sur une VM Windows. Les étapes décrites fonctionnent avec Windows 10 ou 11 64 bits.

## Étape 1 : Prérequis
- Une machine virtuelle disposant d'au moins **8 Go de RAM** et **20 Go d'espace disque**.
- Une connexion internet pour télécharger les dépendances.
- Les droits administrateur sur la VM pour installer les programmes.

## Étape 2 : Installation des dépendances
1. **Visual C++ Redistributable 2019 ou plus récent**
   - Télécharger le package sur le site officiel Microsoft et l'installer.
2. **Git**
   - Installer Git pour Windows afin de récupérer le dépôt.

## Étape 3 : Préparation de la base de données
1. **Installer MySQL ou MariaDB**
   - Utilisez l'installateur officiel et suivez les étapes par défaut.
2. **Créer la base et l'utilisateur**
   - Ouvrir un terminal MySQL :
     ```powershell
     mysql -u root -p
     ```
   - Exécuter les commandes :
     ```sql
     CREATE DATABASE es_extended CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
     CREATE USER 'fivem'@'localhost' IDENTIFIED BY 'votre_mot_de_passe';
     GRANT ALL PRIVILEGES ON es_extended.* TO 'fivem'@'localhost';
     FLUSH PRIVILEGES;
     EXIT;
     ```
3. **Importer le schéma**
   - Depuis le dossier du projet :
     ```powershell
     mysql -u fivem -p es_extended < server-data\[SQL]\es_extended.sql
     ```

## Étape 4 : Récupération du projet
Cloner le dépôt depuis GitHub :
```powershell
git clone <URL-du-dépôt> LunarisV4
```
Placez-vous ensuite dans le dossier cloné :
```powershell
cd LunarisV4
```

## Étape 5 : Téléchargement de l'artifact FiveM
1. Rendez-vous sur [https://runtime.fivem.net/artifacts/fivem/build_server_windows/master/](https://runtime.fivem.net/artifacts/fivem/build_server_windows/master/).
2. Téléchargez la dernière version recommandée.
3. Décompressez l'archive dans un dossier `artifact` à la racine du dépôt.
   Vous devez obtenir `artifact\FXServer.exe`.

## Étape 6 : Configuration du serveur
1. Ouvrir `server-data\server.cfg` et renseigner :
   - `sv_licenseKey "votre_clé_FiveM"` (clé obtenue sur <https://keymaster.fivem.net>). 
   - `mysql_connection_string "mysql://utilisateur:motdepasse@localhost/es_extended?charset=utf8mb4"` en adaptant les identifiants MySQL.
2. Vérifier que `server-data\start.bat` pointe vers le bon emplacement de `FXServer.exe`. Si besoin, modifier le chemin.

## Étape 7 : Lancement
Dans l'invite de commandes :
```powershell
cd server-data
start.bat
```
Ou exécuter directement :
```powershell
..\artifact\FXServer.exe +exec server.cfg +set sv_enforceGameBuild 3407
```
Le serveur se lance alors en écoutant par défaut sur le port **30120**.

## Étape 8 : Connexion au serveur
Depuis le client FiveM, ajoutez un serveur personnalisé à l'adresse `127.0.0.1:30120` (ou l'adresse IP de votre VM si vous y accédez depuis l'extérieur).

---
Votre serveur EmpireTown devrait maintenant être opérationnel sur Windows. Consultez la documentation officielle de FiveM pour plus de détails sur la configuration avancée.
