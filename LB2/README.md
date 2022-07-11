# Einleitung
In diesem Projekt zeigen wir, wie man eine ganze Wordpress Umgebung in Docker umsetzt/aufbaut. <br>
Unser Ziel der Arbeit:
- Wir wollen einen schnellen und einfachen weg um Wordpress aufzusetzen und schritte wie zB. das Installieren von XAMPP etc. vermeiden
- Stattdessen wollen wir unter Docker ein einziges File erstellen und anhand von wenigen Commands die ganze Umgebung aufsetzten


# Inhaltsverszeichnis


# Umsetzung
Also bevor wir anfangen müssen wir Docker installiert haben. Dies kann man ganz einfach über [Docker.com](https://www.docker.com/) machen. <br>
Als nächstes zeigen wir die verschiedenen Images, die wir brauchen werden. Diese können wir im [DockerHub](https://hub.docker.com/) finden. <br>
- [Das offizielle WordPress Image](https://hub.docker.com/_/wordpress): Wir werden das offizielle WordPress Image brauchen. Dieses Image beinhaltet Apache, PHP...
- [MySQL Image](https://hub.docker.com/_/mysql): MySql werden wir auch als Container brauchen. In diesem Image gibt es ein paar Environment Variables wie zB. MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER & MYSQL_PASSWORD (Um einen neuen User zu erstellen)... . Dies sind alles Variablen welche wir in unserem Docker Compose File brauchen werden.
- 





# Testing

# Quellen
Links die wir für die Ideenfindung und für das umsetzen des Projektes gebraucht haben

https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/ <br>
https://www.youtube.com/watch?v=-Hn7vFAr-9s <br>
https://youtu.be/c5c9yVtQGbU <br>
https://www.youtube.com/watch?v=pYhLEV-sRpY <br>
