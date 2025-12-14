# Revrse Proxy Nginx Configuration

Voici la configauration Nginx utilisée pour le reverse proxy, redirigeant les requêtes vers les services appropriés en fonction du nom de domaine.

```bash
sudo nano /etc/nginx/sites-available/sae
```

```nginx
server {
  listen 80;
  server_name gitlab.league.net;

  location / {
    proxy_pass http://127.0.0.1:8080;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}

server {
  listen 80;
  server_name clubphotonailloux.league.net;

  location / {
    proxy_pass http://127.0.0.1:8000;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}
```

Ensuite, activez le site en créant un lien symbolique vers le répertoire `sites-enabled` et rechargez Nginx pour appliquer les modifications :

```bash
sudo ln -s /etc/nginx/sites-available/sae /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```
