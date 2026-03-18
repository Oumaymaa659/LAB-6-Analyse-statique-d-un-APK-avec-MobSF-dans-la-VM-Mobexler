# 🔬 LAB 6 — Analyse Statique d'un APK avec MobSF dans la VM Mobexler

## 📋 Description

Ce laboratoire a pour objectif de réaliser une **analyse statique complète** d'une application Android (APK) à l'aide de l'outil **MobSF (Mobile Security Framework)**, exécuté dans la machine virtuelle **Mobexler**. L'APK cible utilisé est **DIVA (Damn Insecure and Vulnerable App)**, une application volontairement vulnérable conçue à des fins éducatives.

L'analyse statique permet d'inspecter le code source, les permissions, les composants et les configurations d'une application **sans l'exécuter**, afin d'identifier des failles de sécurité potentielles.

---

## 🛠️ Outils Utilisés

| Outil | Rôle |
|-------|------|
| **Mobexler VM** | Environnement d'audit mobile pré-configuré |
| **MobSF** | Framework d'analyse de sécurité mobile (statique & dynamique) |
| **DivaApplication.apk** | Application Android vulnérable utilisée comme cible |
| **sha256sum** | Utilitaire de vérification d'intégrité par empreinte cryptographique |

---

## 📂 Étapes du Laboratoire

---

### 🔐 Étape 1 : Préparation & Vérification de l'Intégrité

**🎯 Objectif :** Garantir que l'APK analysé est **authentique et non corrompu** avant de procéder à l'analyse statique.

Avant toute analyse, il est essentiel de vérifier l'intégrité du fichier APK. Cette vérification repose sur le calcul d'une **empreinte cryptographique SHA-256**. Si le hash correspond à celui attendu, cela confirme que le fichier n'a pas été altéré pendant le téléchargement ou le transfert.

#### 📝 Commandes exécutées :

```bash
# 1. Création de l'arborescence du projet avec la date système
mkdir -p ~/apk_analysis/$(date +%Y-%m-%d)

# 2. Accès au répertoire de travail
cd ~/apk_analysis/$(date +%Y-%m-%d)

# 3. Copie de l'APK depuis le dossier Downloads vers le répertoire d'audit
cp ~/Downloads/DivaApplication.apk ./app-debug.apk

# 4. Calcul de l'empreinte numérique SHA-256 (Fingerprint)
# Cette étape est cruciale pour prouver l'intégrité de l'échantillon
sha256sum app-debug.apk > apk_hash.txt

# 5. Affichage du Hash pour vérification
cat apk_hash.txt
```

#### 📊 Résultat obtenu :

```
5cefc51fce9bd760b92ab2340477f4dda84b4ae0c5d04a8c9493e4fe34fab7c5   app-debug.apk
```

> **✅ Interprétation :** Le hash SHA-256 a été calculé avec succès. Cette empreinte unique permet de confirmer l'intégrité du fichier APK. Toute modification du fichier, même d'un seul octet, produirait un hash complètement différent, garantissant ainsi la fiabilité de l'échantillon analysé.

#### 📸 Capture d'écran :

![Étape 1 — Préparation et vérification de l'intégrité de l'APK](screenshots/etape1_preparation_integrite.png)

---

### 🚀 Étape 2 : Lancement & Analyse Statique avec MobSF

**🎯 Objectif :** Déployer l'instance MobSF et procéder à la **rétro-ingénierie automatisée** de l'APK cible.

MobSF (Mobile Security Framework) est un outil open-source capable de réaliser automatiquement une analyse statique complète d'un APK. Il décompile l'application, extrait les métadonnées, identifie les permissions dangereuses, et attribue un **score de sécurité** global.

#### 📝 Commandes exécutées :

```bash
# Lancement du conteneur Docker dédié à MobSF
# Le script vérifie si le port 8000 est déjà utilisé avant de démarrer
/home/mobexler/AndroidZone/MobSFdocker.sh

# Vérification du statut du conteneur Docker
docker ps
```

#### 🔧 Procédure d'utilisation :

1. **Navigation** vers l'interface web : `http://localhost:8000`
2. **Authentification** avec les identifiants par défaut : `mobsf / mobsf`
3. **Upload** du fichier `app-debug.apk` depuis `~/apk_analysis/2026-03-17/`
4. **Lancement automatique** de l'analyse statique par MobSF

#### 📊 Résultats du Dashboard MobSF :

| Catégorie | Détail |
|-----------|--------|
| **Security Score** | **36/100** ⚠️ (Score faible, indiquant de nombreuses vulnérabilités) |
| **Trackers Detection** | 0/433 |
| **File Name** | DivaApplication.apk |
| **Size** | 1.43 MB |
| **App Name** | Diva |
| **Package Name** | `jakhar.aseem.diva` |
| **Main Activity** | `jakhar.aseem.diva.MainActivity` |
| **Target SDK** | 23 |
| **Min SDK** | 15 |
| **Android Version Name** | 1.0 |
| **Android Version Code** | 1 |

#### 🔑 Empreintes cryptographiques détectées :

| Algorithme | Hash |
|------------|------|
| **MD5** | `82ab8b2193b3cfb1c737e3a786be363a` |
| **SHA1** | `27e849d9d7b86a3a3357fb3e980433a91d416801` |
| **SHA256** | `5cefc51fce9bd760b92ab2340477f4dda84b4ae0c5d04a8c9493e4fe34fab7c5` |

> **✅ Interprétation :** Le score de sécurité de **36/100** est très faible, ce qui est attendu pour DIVA, une application intentionnellement vulnérable. Le hash SHA-256 affiché par MobSF correspond exactement à celui calculé manuellement à l'Étape 1, confirmant ainsi que le fichier analysé est bien le même que celui vérifié initialement. L'absence de trackers (0/433) indique que l'application ne contient pas de bibliothèques de suivi tierces.

#### 📸 Capture d'écran :

![Étape 2 — Dashboard MobSF avec le résultat de l'analyse statique](screenshots/etape2_mobsf_dashboard.png)

---
