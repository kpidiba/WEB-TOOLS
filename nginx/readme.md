# 🐋 NGINX - Guide Complet

## 📖 Qu'est-ce que NGINX ?

**NGINX** (prononcé "engine-x") est un serveur web open-source populaire qui fonctionne également comme un reverse proxy, un équilibreur de charge et un cache HTTP. Il est réputé pour sa haute performance, sa stabilité et sa faible consommation de mémoire.

## ⚙️ Comment fonctionne NGINX ?

### Architecture Événementielle

Contrairement aux serveurs traditionnels (comme Apache) qui utilisent un thread par connexion, NGINX utilise une architecture **asynchrone et événementielle** :

```ts
┌─────────────────────────────────────┐
│         Worker Process              │
│  ┌─────────────────────────────┐    │
│  │     Event Loop              │    │
│  │  ┌─────────────────────┐    │    │
│  │  │  Accept Connection  │────┼────┼──► Nouvelles connexions
│  │  │  Read Request       │    │    │
│  │  │  Process Request    │    │    │
│  │  │  Send Response      │    │    │
│  │  └─────────────────────┘    │    │
│  └─────────────────────────────┘    │
└─────────────────────────────────────┘
```

### Processus Principaux

1. **Master Process** : Gère les configurations et contrôle les workers

2. **Worker Processes** : Gèrent les requêtes client (4 par défaut)

## 🚀 Configuration pour Projets Web

### 📁 Structure de Base des Fichiers

```ts
/etc/nginx/
├── nginx.conf              # Configuration principale
├── sites-available/        # Configurations disponibles
│   └── votre-site.conf
├── sites-enabled/          # Configurations activées
│   └── votre-site.conf -> ../sites-available/votre-site.conf
└── conf.d/                 # Configurations additionnelles
```

## 🛠️ Configuration pour Différents Frameworks

### 1. **Pour Projet Laravel**

```nginx
server {
    listen 80;
    server_name monprojet.local;
    root /var/www/monprojet-laravel/public;
    
    index index.php index.html index.htm;
    
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
    
    location ~ /\.ht {
        deny all;
    }
    
    error_log /var/log/nginx/laravel_error.log;
    access_log /var/log/nginx/laravel_access.log;
}
```

### 2. **Pour Projet React/Angular/Vue (SPA)**

```nginx
server {
    listen 80;
    server_name monapp.local;
    root /var/www/monapp-react/build;
    
    index index.html;
    
    # Gestion des routes SPA
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # Cache pour les assets statiques
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # API proxy si nécessaire
    location /api/ {
        proxy_pass http://localhost:3001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### 3. **Pour Projet Next.js (avec PM2 ou système similaire)**

```nginx
server {
    listen 80;
    server_name nextapp.local;
    
    # Reverse proxy vers l'application Next.js
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
    
    # Cache pour les assets statiques
    location /_next/static/ {
        alias /var/www/nextapp/.next/static/;
        expires 365d;
        access_log off;
    }
}
```

## 🌐 Utiliser des Sous-domaines en Local

### Méthode 1 : Modifier le fichier `/etc/hosts`

1. **Éditez le fichier hosts** (nécessite les droits admin) :
   
   ```bash
   # Sur Linux/Mac
   sudo nano /etc/hosts
   
   # Sur Windows
   # C:\Windows\System32\drivers\etc\hosts
   ```

2. **Ajoutez les entrées** :
   
   ```hosts
   127.0.0.1       api.monprojet.local
   127.0.0.1       app.monprojet.local
   127.0.0.1       admin.monprojet.local
   127.0.0.1       monprojet.local
   ```

### Méthode 2 : Configuration NGINX avec Wildcard

```nginx
# Configuration pour les sous-domaines
server {
    listen 80;
    server_name ~^(?<subdomain>.+)\.monprojet\.local$;
    root /var/www/monprojet/$subdomain/public;
    
    index index.php index.html;
    
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    
    # ... autres configurations selon le framework
}
```

### Configuration Pratique avec Plusieurs Projets

```nginx
# Projet principal
server {
    listen 80;
    server_name monprojet.local;
    root /var/www/monprojet/public;
    # ... configuration Laravel
}

# API sous-domaine
server {
    listen 80;
    server_name api.monprojet.local;
    root /var/www/monprojet-api/public;
    
    location / {
        proxy_pass http://localhost:8000;
        # Headers pour API
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Methods "GET, POST, PUT, DELETE, OPTIONS";
    }
}

# Application frontend
server {
    listen 80;
    server_name app.monprojet.local;
    root /var/www/monprojet-frontend/build;
    # ... configuration SPA
}

# Admin panel
server {
    listen 80;
    server_name admin.monprojet.local;
    root /var/www/monprojet-admin/public;
    # ... configuration Laravel/autre
}
```

## 🔧 Configuration SSL en Local (HTTPS)

1. **Générer un certificat auto-signé** :
   
   ```bash
   # Créer un répertoire pour les certificats
   sudo mkdir -p /etc/nginx/ssl
   
   # Générer la clé privée et le certificat
   sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
       -keyout /etc/nginx/ssl/monprojet.key \
       -out /etc/nginx/ssl/monprojet.crt \
       -subj "/C=FR/ST=Paris/L=Paris/O=MonProjet/CN=monprojet.local"
   ```

2. **Configuration NGINX avec SSL** :
   
   ```nginx
   server {
       listen 443 ssl http2;
       server_name monprojet.local;
       
       ssl_certificate /etc/nginx/ssl/monprojet.crt;
       ssl_certificate_key /etc/nginx/ssl/monprojet.key;
       
       root /var/www/monprojet/public;
       # ... reste de la configuration
   }
   
   server {
       listen 80;
       server_name monprojet.local;
       return 301 https://$server_name$request_uri;
   }
   ```

## 🚀 Script d'Automatisation

```bash
#!/bin/bash
# create-nginx-site.sh

SITE_NAME=$1
DOMAIN="$SITE_NAME.local"
ROOT_DIR="/var/www/$SITE_NAME"
CONF_FILE="/etc/nginx/sites-available/$DOMAIN"

# Créer le répertoire du projet
sudo mkdir -p $ROOT_DIR

# Créer la configuration NGINX
sudo tee $CONF_FILE > /dev/null <<EOF
server {
    listen 80;
    server_name $DOMAIN;
    root $ROOT_DIR/public;
    
    index index.php index.html index.htm;
    
    location / {
        try_files \$uri \$uri/ /index.php?\$query_string;
    }
    
    location ~ \.php\$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
    }
}
EOF

# Activer le site
sudo ln -s $CONF_FILE /etc/nginx/sites-enabled/

# Ajouter au fichier hosts
echo "127.0.0.1    $DOMAIN" | sudo tee -a /etc/hosts

# Tester et recharger NGINX
sudo nginx -t && sudo systemctl reload nginx

echo "✅ Site $DOMAIN créé avec succès!"
echo "📁 Répertoire: $ROOT_DIR"
echo "🌐 Accès: http://$DOMAIN"
```

## 🐛 Debugging Common Issues

### Vérifier la syntaxe de configuration :

```bash
sudo nginx -t
```

### Voir les logs :

```bash
# Logs d'erreur
sudo tail -f /var/log/nginx/error.log

# Logs d'accès
sudo tail -f /var/log/nginx/access.log

# Logs spécifiques au site
sudo tail -f /var/log/nginx/monprojet_error.log
```

### Problèmes de permissions :

```bash
sudo chown -R www-data:www-data /var/www/monprojet
sudo chmod -R 755 /var/www/monprojet/storage
sudo chmod -R 755 /var/www/monprojet/bootstrap/cache
```

## 📚 Bonnes Pratiques

1. **Sécurité** :
   
   - Toujours limiter l'accès aux fichiers sensibles
   
   - Utiliser des permissions strictes
   
   - Mettre à jour NGINX régulièrement

2. **Performance** :
   
   - Activer gzip compression
   
   - Configurer le cache pour les assets statiques
   
   - Optimiser les timeouts

3. **Maintenance** :
   
   - Organiser les configurations par site
   
   - Utiliser des noms de fichiers descriptifs
   
   - Documenter les configurations complexes

## 🔗 Ressources Utiles

- [Documentation Officielle NGINX](https://nginx.org/en/docs/)

- [NGINX Configuration Generator](https://www.digitalocean.com/community/tools/nginx)

- [SSL Configuration Generator](https://ssl-config.mozilla.org/)

Ce guide vous permet de configurer NGINX pour différents types de projets et d'utiliser des sous-domaines en local pour un environnement de développement plus réaliste.


