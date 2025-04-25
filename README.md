Option 1: NGINX Proxy Manager (Empfohlen)
Ein spezielles Docker-Image mit eingebautem Web-Interface für die Verwaltung:

yaml
version: '3.8'

services:
  proxy-manager:
    image: jc21/nginx-proxy-manager
    container_name: proxy-manager
    ports:
      - "80:80"
      - "443:443"
      - "8181:81"  # Admin-Oberfläche
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    networks:
      - proxy-network
    restart: unless-stopped

  wordpress:
    image: wordpress:php8.3-apache
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: secure_password
    volumes:
      - wordpress_data:/var/www/html
    networks:
      - proxy-network
      - internal-network
    depends_on:
      - db

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root_secret
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: secure_password
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - internal-network

volumes:
  wordpress_data:
  db_data:
  data:
  letsencrypt:

networks:
  proxy-network:
  internal-network:
Verwendung:

Admin-GUI erreichen unter: http://localhost:8181

Standard-Login:

Email: admin@example.com

Password: changeme
```
