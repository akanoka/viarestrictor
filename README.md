# ViaRestrictor

**Bloquez certaines versions de Minecraft (via ViaVersion) et affichez un message personnalisé au joueur.**

---

## Table des matières

* [Présentation](#présentation)
* [Fonctionnalités](#fonctionnalités)
* [Prérequis](#prérequis)
* [Installation](#installation)
* [Configuration](#configuration)
* [Utilisation](#utilisation)
* [Développement & Build](#développement--build)
* [Contribuer](#contribuer)
* [Licence](#licence)
* [Crédits](#crédits)

---

## Présentation

ViaRestrictor est un plugin Java pour serveurs Minecraft (Spigot/Paper) qui s'appuie sur **ViaVersion** pour détecter la version client d'un joueur et, selon la configuration, empêcher la connexion et/ou afficher un message personnalisé.

Il est utile pour :

* Forcer une plage de versions autorisées.
* Bloquer des versions connues pour causer des problèmes ou être utilisées par des clients non désirés.

---

## Fonctionnalités

* Définir une liste de versions **interdites** ou une liste **autorisée**.
* Message personnalisable envoyé au joueur tentant de se connecter avec une version non autorisée (supporte codes de couleur Minecraft `&`).
* Option pour `kick` le joueur automatiquement.
* Intégration détectant la présence de ViaVersion au démarrage.

---

## Prérequis

* Java 17+ (ou la version recommandée pour votre serveur)
* Serveur Spigot/Paper compatible
* ViaVersion installé (requis pour détecter correctement les versions client)

---

## Installation

1. Téléchargez le JAR (`ViaRestrictor.jar`) depuis les releases GitHub ou compilez depuis la source.
2. Placez `ViaRestrictor.jar` dans le dossier `plugins/` de votre serveur.
3. Redémarrez le serveur. Le plugin créera automatiquement `plugins/ViaRestrictor/config.yml`.

---

## Configuration

Un fichier `config.yml` est généré au premier démarrage. Voici un exemple utilisable :

```yaml
# ViaRestrictor configuration
mode: "blacklist" # "blacklist" pour interdire les versions listées, "whitelist" pour n'autoriser que les versions listées
versions:
  - "1.8"
  - "1.12.2"
  - "1.20.4"
message: "&cDésolé, votre version de Minecraft n'est pas prise en charge.\n&7Mettez à jour votre client ou contactez un administrateur."
kick: true
log-blocked: true # log les tentatives de connexion bloquées dans la console

# Exemple avancé : bloquer toutes les versions < 1.16 (utilisé côté plugin dans la logique)
# Vous pouvez aussi utiliser des ranges dans le code si nécessaire.
```

**Champs importants :**

* `mode` : `blacklist` (versions listées interdites) ou `whitelist` (seules versions listées autorisées).
* `versions` : tableau de chaînes représentant des versions clients.
* `message` : message envoyé au joueur (supporte `\n` pour sauts de ligne et codes couleurs `&`).
* `kick` : si `true`, le joueur est expulsé après affichage du message.
* `log-blocked` : si `true`, les tentatives bloquées sont loggées côté console.

---

## Utilisation

* Modifier `config.yml` selon vos besoins.
* Redémarrer le serveur ou recharger la configuration (si vous implémentez une commande `/vr reload`).
* Testez la connexion avec différentes versions pour vérifier le comportement.

### Commandes (suggestions)

> Le plugin n'inclut pas forcément ces commandes par défaut — c'est une suggestion d'API à implémenter.

```
/vr reload   # recharge la configuration
/vr status   # affiche l'état (ViaVersion détecté, mode, nombre de versions configurées)
/vr add <version>    # ajoute une version à la liste
/vr remove <version> # retire une version de la liste
```

---

## Développement & Build

### Structure recommandée

```
src/main/java/akanoka/viarestrictor/
  - ViaRestrictor.java (main class extends JavaPlugin)
  - VersionChecker.java (logique de détection et comparaison de versions)
  - ConfigManager.java (lecture/écriture config.yml)
  - Commands/ (si vous ajoutez commandes)
resources/
  - plugin.yml
  - config.yml (exemple)
```

### Build (Maven)

Exemple de `pom.xml` minimal :

```xml
<!-- ajoutez les dépendances Spigot/Paper et ViaVersion si nécessaire -->
```

Pour compiler :

```bash
mvn clean package
```

---

## Contribuer

Contributions bienvenues — issues, suggestions et pull requests. Merci de suivre ces règles :

1. Fork et créez une branche pour chaque feature ou bug.
2. Tests et compatibilité avec ViaVersion appréciés.
3. Documentez toute modification majeure dans le README.

---

## Licence

Ce projet est sous licence **Apache License 2.0**. Voir le fichier `LICENSE` pour le texte complet.

---

## Crédits

* Maintenu par AkaNoka
* Inspiré par les besoins des serveurs Paper/Spigot pour garder des versions client homogènes

---

*Dernière mise à jour : 31 Octobre 2025*
