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
