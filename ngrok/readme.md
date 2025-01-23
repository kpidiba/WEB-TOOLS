# 🚀 Ngrok - Votre Tunnel vers le Monde

Ngrok est un outil incroyable qui permet d'exposer des applications locales à Internet via des tunnels sécurisés. C'est une solution idéale pour les développeurs qui souhaitent tester ou partager leurs projets localement sans avoir à configurer un serveur complexe.  

---

## 📥 Téléchargement et Installation

### 🌐 1. Téléchargement

Rendez-vous sur le site officiel pour télécharger la version adaptée à votre système d'exploitation :  
🔗 [Télécharger Ngrok](https://ngrok.com/download)  

### 🖥️ 2. Installation

#### 🐧 Linux

```bash
# Téléchargez Ngrok
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-stable-linux-amd64.zip

# Décompressez l'archive
unzip ngrok-stable-linux-amd64.zip

# Déplacez l'exécutable vers un chemin global
sudo mv ngrok /usr/local/bin
```

#### 🍎 macOS

```bash
# Installation via Homebrew
brew install ngrok
```

#### 🪟 Windows

1. Téléchargez le fichier `.zip` depuis ce lien [download ngrok]([Download ngrok](https://download.ngrok.com/downloads/windows)).
2. Décompressez l'archive dans C:\ngrok.
3. Ajoutez le fichier `ngrok.exe` au `PATH` pour l'utiliser dans le terminal.

---

## 🔑 Authentification de votre compte

Après l'installation, vous devez connecter votre compte Ngrok pour utiliser toutes ses fonctionnalités :

1. Connectez-vous ou créez un compte sur [ngrok.com](https://ngrok.com).

2. Récupérez votre **authtoken** dans la section "Setup Instructions".

3. Configurez Ngrok avec votre token :

```bash
ngrok config add-authtoken VOTRE_AUTHTOKEN
```

---

## 💻 Utilisation de Ngrok

### 1. Exposer un serveur HTTP local

Si vous avez une application locale qui tourne sur un port (exemple : `http://localhost:8000`), exécutez :

```bash
ngrok http 8000
```

Cela génère une URL publique, par exemple : `https://random.ngrok.io`.

Pour résoudre les problèmes de chargement des ressources en HTTPS, ajoutez la balise meta suivante dans votre fichier HTML :

```html
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">
```

### 2. Exposer un serveur TCP

Pour partager un service TCP (exemple : un serveur SSH sur le port 22), utilisez :

```bash
ngrok tcp 22
```

---

## 🔄 Utilisation avec des Frameworks

### ⚡ Laravel

Ngrok est idéal pour exposer un projet Laravel local à des services tiers, comme Stripe, PayPal ou des API webhook.

#### Étapes :

1. **Lancer le serveur Laravel** :

```bash
php artisan serve
```

Par défaut, votre application est disponible sur `http://127.0.0.1:8000`.

**Démarrer Ngrok** :

```bash
ngrok http 8000
```

Cela génère une URL publique que vous pouvez utiliser pour tester vos webhooks, par exemple :  
`https://random.ngrok.io`.

**Configurer votre application Laravel** :  
Dans le fichier `.env`, mettez à jour la variable `APP_URL` avec l'URL générée par Ngrok :

```env
APP_URL=https://random.ngrok.io
```

**Tester vos webhooks** :  
Configurez vos services tiers (Stripe, PayPal, etc.) pour utiliser l'URL publique générée par Ngrok.

---

### ⚙️ Node.js (Express)

1. Lancez votre serveur Express

```bash
node app.js
```

2. Exposez le serveur avec Ngrok :

```bash
ngrok http 3000
```

3. Utilisez l'URL publique générée pour vos tests ou partages.

### 📦 Autres Frameworks

Ngrok fonctionne également avec des frameworks comme :

- Django (Python)
- Spring Boot (Java)
- Ruby on Rails
- Flask

L'approche est la même : démarrez votre serveur local, puis exposez-le avec Ngrok.

## 📊 Interface Web

Ngrok fournit une interface web pratique pour surveiller les requêtes entrantes. Par défaut, elle est accessible à l'adresse :

```bash
http://127.0.0.1:4040
```

---

## 🛡️ Sécurité

Ngrok offre des fonctionnalités avancées pour sécuriser vos tunnels, comme :

- Authentification via mot de passe.
- Restrictions IP.
- HTTPS natif.

Exemple pour ajouter une authentification basique :

```bash
ngrok http --basic-auth="username:password" 8000
```

---

## 📚 Ressources utiles

- Site officiel : [ngrok.com](https://ngrok.com)
- Documentation complète : docs.ngrok.com

---

## 🌟 Contribuez

Si vous avez des suggestions pour améliorer ce guide, n'hésitez pas à ouvrir une [issue](https://github.com/username/repository/issues) ou une pull request.

---

> 🚀 **N'oubliez pas de donner une étoile ⭐ à ce dépôt si vous le trouvez utile ! Ce guide est complet, couvrant tout, de l'installation à l'utilisation avec Laravel et d'autres frameworks. Vous pouvez personnaliser les sections ou ajouter des détails spécifiques selon vos besoins.**
