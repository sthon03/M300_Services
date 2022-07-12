# Einleitung
In diesem Projekt zeigen wir, wie man eine ganze Wordpress mit Mysql und PHPmyadmin Umgebung in Docker umsetzt/aufbaut. <br>
Unser Ziel der Arbeit:
- Wir wollen einen schnellen und einfachen weg um Wordpress aufzusetzen und schritte wie zB. das Installieren von XAMPP etc. vermeiden
- Stattdessen wollen wir unter Docker ein einziges File erstellen und anhand von wenigen Commands die ganze Umgebung aufsetzten
<br><br>

![Wordpress und Docker Abbildung](/LB2/images/docker-wordpress.png)

<br><br>

Zuerst haben wir versucht unsere Wordpress Umgebung in Kubernetes aufzusetzen, haben aber jedoch bemerkt, das dies zu anspruchsvoll ist und wir es mit unserem jetzigen Know-How nicht rechtzeitig schaffen würde aufzusetzen. Deswegen haben wir uns entschieden, den Auftrag doch mit Docker umzusetzen und in weiterer Zukunft, diesen Auftrag in Kubernetes einzurichten. Wir haben es mit diesem Video versucht umzusetzen: https://www.youtube.com/watch?v=-Hn7vFAr-9s&t=151s
<br>

Zuerst müssen wir eine pvc .yaml Datei für Kubernetes erstellen mit diesem Inhalt:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: wordpress-volume
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: block-storage-class-name
```
Anschliessend führen wir die Datei aus mit:

```
kubectl apply -f wp-volume.yaml
```
Dann brauchen wir eine .yml Datei für das Wordpress Pod:

````
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
        - name: wordpress
          image: wordpress:5.8.3-php7.4-apache
          ports:
          - containerPort: 80
            name: wordpress
          volumeMounts:
            - name: wordpress-data
              mountPath: /var/www
          env:
            - name: WORDPRESS_DB_HOST
              value: mysql-service.default.svc.cluster.local
            - name: WORDPRESS_DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: wp-db-secrets
                  key: MYSQL_ROOT_PASSWORD
            - name: WORDPRESS_DB_USER
              value: root
            - name: WORDPRESS_DB_NAME
              value: wordpress
      volumes:
        - name: wordpress-data
          persistentVolumeClaim:
            claimName: wordpress-volume
````

Ebenso führen wir auch diese Datei aus mit:

````
kubectl apply -f wp.yaml
````

Noch eine letzte .yml Konfigurationsdatei müssen wir erstellen und ausführen und dann sind wir so weit, um auf unseren Wordpress zuzugreifen:

````
---
kind: Service
apiVersion: v1
metadata:
  name: wordpress-service
spec:
  type: LoadBalancer
  selector:
    app: wordpress
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
````

Ebenso führen wir diese letzte Datei noch aus:

````
kubectl apply -f wp-service.yaml
````
Nun sollte man auf die Wordpress Konfigurationsseite zugreifen können, indem man die Externe-IP in den Browser eingibt, bei uns ist jedoch keine externe IP vorhanden und somit kann man nicht auf die Konfiguration zugreifen.

![Kubernetes Pod](/LB2/images/KubernetesPodWordpress.png)

<br>

Im ganzen hat uns hat das nötige Know-How gefehlt und wir mussten kurzfristig auf Docker umsteigen, da wir diese Containervirtualisierungssoftware einigermassen verstanden haben. 

![Kubernetes und Wordpress Abbildung](/LB2/images/KubernetesWordpress.png)

# Umsetzung

## Teil 1 | Umsetzung

<br>

Also bevor wir anfangen müssen wir Docker installiert haben. Dies kann man ganz einfach über [Docker.com](https://www.docker.com/) machen. <br>
Als nächstes zeigen wir die verschiedenen Images, die wir brauchen werden. Diese können wir im [DockerHub](https://hub.docker.com/) finden. <br>
- [Das offizielle WordPress Image](https://hub.docker.com/_/wordpress): Wir werden das offizielle WordPress Image brauchen. Dieses Image beinhaltet Apache, PHP...
- [MySQL Image](https://hub.docker.com/_/mysql): MySql werden wir auch als Container brauchen. In diesem Image gibt es ein paar Environment Variables wie zB. MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER & MYSQL_PASSWORD (Um einen neuen User zu erstellen)... . Dies sind alles Variablen welche wir in unserem Docker Compose File brauchen werden.
- [Official phpMyAdmin Docker image](https://hub.docker.com/r/phpmyadmin/phpmyadmin): Dieses Image wird uns dazu dienen um Tabellen, Spalten etc. zu sehen.

Das Ziel wäre es also ein File zusammenzustellen, welches in der Lage ist all diese Dinge für uns aufzusetzen. <br>
Wenn soweit alles richtig installiert wurde können wir mit dem Erstellen des Compose Files anfangen.<br>

Neues Compose File erstellen: <br>
WICHTIG: Das Compose File muss eine .yaml Endung besitzen, damit es als YAML File erkennt wird.  <br>
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
      - '8888:80'
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
WICHTIG: Ebenfalls sollte man aufpassen, das die angegebenen Ports nicht belegt sind, ansonsten kann man nicht auf die Wordpress oder PHPmymadmin Seite zugreifen. Deswegen haben wir für unsere PHPmyadmin Seite, den Port 8888 gegeben, weil 8080 vergeben war. 

<br>

## Teil 2 | Umsetzung
<br>
Nun müssen wir unser .yaml File auf unserem Server per FTP rübertransferieren, weil wir es sonst Lokal ausführen würden und das wollen wir nicht. 

Um dies auszuführen müssen wir diesen Befehl eingeben:
```
sudo wget https://raw.githubusercontent.com/sthon03/M300_Services/main/docker-compose.yml
```
![sudo wget Befehl](/LB2/images/wget_Befehl.png)

Nun müssen wir nur noch mit compose unsere Datei ausführen und die Container einrichten. 

Dazu geben wir diesen Befehl ein:

```
sudo docker-compose up -d
```
![docker compose befehl](/LB2/images/dockerComposeUp_Befehl.png)

jetzt müssen wir uns ein bisschen gedulden, bis die Container eingerichtet werden und wir auf unsere Webseite zugreifen können.

Um die Container zu stoppen und entfernen, weil man Speicherplatz braucht, führt man diesen Befehl aus:
```
sudo docker-compose down --rmi all
```
![docker compose down Befehl](/LB2/images/dockerComposeDown_Befehl.png)

Wir machen dies aber jetzt nicht, weil wir unsere Wordpress Seite noch fertigstellen wollen.

## Teil 3 | Umsetzung
<br>
Nachdem wir unsere Container aufgesetzt haben versuchen wir im "Google Chrome" auf die Seite zuzugreifen, indem wir unsere IP-Adresse eingeben mit dem Port dahinter.

`
10.3.43.26:8000
`
<br>

Es sollt euch die Wordpress Standardseite begrüssen. Falls dies der Fall ist, dann habt ihr bis jetzt alles richtig gemacht.

![Wordpress Zugriff](/LB2/images/WordpressStart.png)

*Anschliessend müssen wir noch ein paar Konfigurationsparameter einrichten.*

![Wordpress Config](/LB2/images/WordpressConfig.png)

*Die vorherigen Konfigurationsparameter braucht man nun, um sich im Wordpress einzuloggen.*

![Wordpress Login](/LB2/images/WordpressLogin.png)

*Nun sollte man auf diese Startseite landen. Falls dies der Fall ist, dann hat man alles richtig konfiguriert.*

![Wordpress Startseite](/LB2/images/StartseiteWordpress.png)

Wir versuchen nun mit der IP und mit dem Port:

`
10.3.43.26:8888
`

die PHPmyadmin Seite zu öffnen und unsere Datenbanken anzuschauen. 

![PHPmyadmin Webseite](/LB2/images/PHPmyadminDatenbank.png)

<br>
<br>

# Testing

Unten sind unsere Testcases in Tabellenform beschrieben. 

![TestCases](/LB2/images/TestCases1.png)



![TestCases2](/LB2/images/TestCases2.png)

# Quellen
Links die wir für die Ideenfindung und für das Umsetzen des Projektes gebraucht haben:

<br>

https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/ <br>
https://www.youtube.com/watch?v=-Hn7vFAr-9s <br>
https://youtu.be/c5c9yVtQGbU <br>
https://www.youtube.com/watch?v=pYhLEV-sRpY <br>
https://youtu.be/pYhLEV-sRpY <br>