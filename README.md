# 🌐 Web Tools Repository

Bienvenue dans le **Web Tools Repository** ! Ce dépôt centralise des outils indispensables pour les développeurs web, comme [Ngrok](https://ngrok.com), et fournit des détails sur leur utilisation. 🌟  

---

## 📝 Rôle de ce README

Le rôle principal de ce fichier est de :  

1. **Présenter le projet** et ses objectifs.  
2. **Lister les outils disponibles** avec des explications claires.  
3. Servir de **documentation rapide** pour les utilisateurs.  

---

## 🛠️ Outils disponibles

Voici la liste des outils inclus dans ce dépôt :  

### 1. 🚀 [Ngrok](./ngrok/readme.md)

**Ngrok** est un outil puissant qui vous permet d'exposer un serveur local à Internet via un tunnel sécurisé. Il est particulièrement utile pour :  

- Tester des applications localement avec des services tiers (webhooks, etc.).  
- Partager un serveur local avec des collègues ou des clients.  
- Debugger des API en direct.  

#### 📚 Documentation rapide :

- **Site officiel** : [ngrok.com](https://ngrok.com)  

- **Installation** :  
  
  ```bash
  # Pour Linux
  sudo apt update && sudo apt install ngrok  
  
  # Pour MacOS avec Homebrew
  brew install ngrok  
  
  # Pour Windows
  # Télécharger le binaire depuis : https://download.ngrok.com/downloads/
  ```

## 2. [COOLORS](https://coolors.co/)

**Coolors** is a fast and intuitive color palette generator loved by designers and developers. Whether you're creating a new brand, designing a website, or refreshing your UI, Coolors helps you discover perfect color combinations in seconds. You can explore trending palettes, adjust hues with precision, and even export in multiple formats (PNG, SCSS, SVG, etc.). Ideal for inspiring creativity and maintaining color consistency in any project.

---

### 3. 🐋 NGINX

**NGINX** est un serveur web haute performance, un reverse proxy et un équilibreur de charge. Il est largement utilisé pour :

- Servir des sites web statiques et dynamiques

- Gérer le load balancing entre plusieurs serveurs backend

- Configurer un reverse proxy pour des applications web

- Gérer le cache HTTP et la compression

- Sécuriser les applications avec SSL/TLS

#### 📚 Documentation rapide :

- **Site officiel** : [nginx.org](https://nginx.org/)

- **Installation** :

```bash
# Pour Ubuntu/Debian
sudo apt update
sudo apt install nginx

# Pour CentOS/RHEL
sudo yum install epel-release
sudo yum install nginx

# Pour MacOS avec Homebrew
brew install nginx

# Pour Windows
# Télécharger depuis : https://nginx.org/en/download.html
```

**Commandes utiles** :

```bash
# Démarrer NGINX
sudo systemctl start nginx

# Arrêter NGINX
sudo systemctl stop nginx

# Redémarrer NGINX
sudo systemctl restart nginx

# Recharger la configuration sans interruption
sudo systemctl reload nginx

# Vérifier l'état
sudo systemctl status nginx

# Tester la configuration
sudo nginx -t
```

**Fichiers de configuration principaux** :

```json
/etc/nginx/nginx.conf           # Configuration principale
/etc/nginx/sites-available/     # Sites disponibles
/etc/nginx/sites-enabled/       # Sites activés
/var/log/nginx/                 # Fichiers de logs
```



## 🙌 Contributions

Ce projet est ouvert aux contributions ! Si vous avez des suggestions ou souhaitez ajouter d'autres outils, n'hésitez pas à soumettre une [issue](https://github.com/username/repository/issues) ou une pull request.

---

## 📧 Contact

Pour toute question ou suggestion :

- ✉️ Email : kpidibadavid1@gmail.com
- 💼 GitHub : [MON PROFIL](https://github.com/kpidiba)

---

> ⭐ **N'oubliez pas de donner une étoile à ce dépôt si vous le trouvez utile !**
