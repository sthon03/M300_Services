# Einleitung
In diesem Projekt zeigen wir, wie man eine ganze Wordpress Umgebung in Docker umsetzt/aufbaut. <br>
Unser Ziel der Arbeit:
- Wir wollen einen schnellen und einfachen weg um Wordpress aufzusetzen und schritte wie zB. das Installieren von XAMPP etc. vermeiden
- Stattdessen wollen wir unter Docker ein einziges File erstellen und anhand von wenigen Commands die ganze Umgebung aufsetzten

# Umsetzung
Also bevor wir anfangen müssen wir Docker installiert haben. Dies kann man ganz einfach über [Docker.com](https://www.docker.com/) machen. <br>
Als nächstes zeigen wir die verschiedenen Images, die wir brauchen werden. Diese können wir im [DockerHub](https://hub.docker.com/) finden. <br>
- [Das offizielle WordPress Image](https://hub.docker.com/_/wordpress): Wir werden das offizielle WordPress Image brauchen. Dieses Image beinhaltet Apache, PHP...
- [MySQL Image](https://hub.docker.com/_/mysql): MySql werden wir auch als Container brauchen. In diesem Image gibt es ein paar Environment Variables wie zB. MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER & MYSQL_PASSWORD (Um einen neuen User zu erstellen)... . Dies sind alles Variablen welche wir in unserem Docker Compose File brauchen werden.
- [Official phpMyAdmin Docker image](https://hub.docker.com/r/phpmyadmin/phpmyadmin): Dieses Image wird uns dazu dienen um Tabellen, Spalten etc. zu sehen.

Das Ziel wäre es also ein File zusammenzustellen, welches in der Lage ist all diese Dinge für uns aufzusetzen. <br>
Wenn soweit alles richtig installiert wurde können wir mit dem Erstellen des Compose Files anfangen.<br>

Neues Compose File erstellen: <br>
WICHTIG: Das Compose File muss eine .yaml Endung besitzen, damit es als YAML File erkennt wird <br>
````
touch docker-compose.yaml
````

Unser Compse File ist folgendermassen aufgebaut:
````
version: '3'

services:
  # Database
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks:
      - wpsite
  # phpmyadmin
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - '8080:80'
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: password 
    networks:
      - wpsite
  # Wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - '8000:80'
    restart: always
    volumes: ['./:/var/www/html']
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    networks:
      - wpsite
networks:
  wpsite:
volumes:
  db_data:
````
Configuartion der Container in Betrieb nehmen:
````
$ docker-compose up -d
````
Configuartion der Container herunterfahren
````
$ docker-compose down -volumes
````

# Testing

# Quellen
Links die wir für die Ideenfindung und für das Umsetzen des Projektes gebraucht haben:<br>
https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/ <br>
https://www.youtube.com/watch?v=-Hn7vFAr-9s <br>
https://youtu.be/c5c9yVtQGbU <br>
https://www.youtube.com/watch?v=pYhLEV-sRpY <br>
