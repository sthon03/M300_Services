# Einleitung
In diesem Repository werden meine Aufträge und Kenntnisse vom Modul 300 dokumentiert und erklärt. Im Modul haben wir hauptsächlich die Themen Vagrant, Docker und Kubernetes behandelt.

# Handlungskompetenzen von Modul 300
**1.Ermittelt aus Vorgaben die erforderlichen Services und leitet nötige Anforderungen ab.**

**Handlungsnotwendige Kenntnisse:**
1.Kennt die Einsatz- und Konfigurationsmöglichkeiten von vorgegebenen Betriebssystemen und Services.
2.Kennt die Vorgehensweise für die Analyse von Anforderungen.

**2.Erarbeitet ein Konzept für die Integration der Services gemäss Vorgaben und definiert Sicherheitsmassnamen.**

**Handlungsnotwendige Kenntnisse:**
1.Kennt die wichtigsten inhaltlichen und formalen Regeln, die bei der Erstellung eines Konzeptes einzuhalten sind.
2.Kennt die Vorgehensweise zur Definition von Testfällen.
3.Kennt die wichtigsten Schnittstellen und ihre Eigenschaften innerhalb einer heterogenen Systemumgebung.
4.Kennt die anzuwendenden Schutz- und Sicherheitsmassnahmen für Services.

**3.Konfiguriert die benötigten Services gemäss Vorgaben und überprüft deren Funktionalität anhand der Anforderungen.**

**Handlungsnotwendige Kenntnisse:**
1.Kennt die übliche Vorgehensweise bei der Inbetriebnahme von Services (z.B. installieren, starten, testen).
2.Kennt betriebssystemspezifische Konzepte zur Konfiguration von Services (z.B. Konfigurationsdateien, Registry, systemweite/benutzerspezifische Konfiguration).
3.Kennt die unterschiedlichen Konzepte, Systembefehle und Hilfsprogramme für die Benutzer- und Rechteverwaltung (z.B. User-ID, Zugriffsrechte, Gruppenmitgliedschaft, Standardrechte, Vererbung, Homeverzeichnis).
4.Kennt technische Möglichkeiten, um Ressourcen im Netzwerk durch Gruppen gemeinsam zu nutzen.
5.Kennt Testmöglichkeiten verschiedener Services.

**4.Konfiguriert Netzwerkverbindungen und überprüft deren Funktionalität.**

**Handlungsnotwendige Kenntnisse:**
1.Kennt die Elemente und Funktionen eines TCP/IP-Protokolls (z.B. MAC- und IP-Adressen, IP-Adressklassen, Netzmasken, Routing, Adress Resolution Protocol, TCP/UDP, wichtige Portnummern).
2.Kennt die Konfigurationsmöglichkeiten bei Netzwerkverbindungen.
3.Kennt Möglichkeiten, um die Netzwerkverbindungen zu testen.

**5.Integriert verschiedene Services und Plattformen zu einem Gesamtsystem und testet die Integration.**

**Handlungsnotwendige Kenntnisse:**
1.Kennt die Funktionsweisen der anzuwendenden Services (z.B. DHCP, DNS, Verzeichnisdienste, Fileservices, Printservices).
2.Kennt die Konfigurationsmöglichkeiten der anzuwendenden Services.

**6.Grenzt allfällige Fehler systematisch ein, protokolliert diese und leitet Massnahmen zur Fehlerbehebung ein.**

**Handlungsnotwendige Kenntnisse:**
1.Kennt den Aufbau und die wesentlichen Merkmale eines Testprotokolls.
2.Kennt verschiedene Testmethoden für die Funktionalität der Services (z.B. Blackbox, White-box).
3.Kennt Methoden zur systematischen Fehlereingrenzung (z.B. Ausschlussverfahren intakter Systeme).
4.Kennt Werkzeuge zur Fehleranalyse und Fehlerbehebung.

**7.Dokumentiert das Gesamtsystem so, dass es jederzeit nachvollzogen werden kann.**

**Handlungsnotwendige Kenntnisse:**
1.Kennt die Bedeutung einer Dokumentation zur Sicherstellung und Nachvollziehbarkeit von Arbeitsergebnissen.
2.Kennt die wichtigsten inhaltlichen und formalen Regeln, die bei der Dokumentation von Arbeitsergebnissen einzuhalten sind.

# Zeitplan M300
![image](https://user-images.githubusercontent.com/105722466/176194961-a8cc1e85-a7cc-4176-ba11-271ca542b1a1.png)


# Hauptteil
Nun kommt der Hauptteil der des Repositorys

## Spick LB01
Hier ist noch die Zusammenfassung, welche ich zur Vorbereitung von LB01 zusammengetragen habe:
![image](https://user-images.githubusercontent.com/105722466/176195599-e002aaaf-6c2e-4ac3-a290-aeb3763f2e39.png)
![image](https://user-images.githubusercontent.com/105722466/176195630-7954874a-be75-411d-b7a4-515203bd3958.png)


## Aufbau der TBZ Infrastruktur
In der untenstehenden Grafik wird erklärt, wie man sich die TBZ-Cloud verbindet:

![image](https://user-images.githubusercontent.com/105722466/176201601-212b8e47-7e0c-4142-8305-ead0cf29f2d1.png)

**Der Aufbau besteht aus den folgenden Komponenten:**
-	Laptop des Lernenden
-	Linux VM in der TBZ-Cloud (fungiert als Host)
-	Guest als Vagrant (VM in VM)

## Virtualisierung
In der unteren Grafik wird das movement von Baremetal zu Virtualisierung 1.0 und schliesslich zu Virtualisierung 2.0 über die vergangen Jahre erklärt.

**Vorteile von Cloudlösungen**
- Tiefere Lizenzkosten
- Weniger administrationsaufwand
- Tiefere Produktionskosten
- Hohe Flexibilität
- Redundanz, Unabhängigkeit

**Nachteile von Cloudlösungen**
- Gemeinsamer Kernel von Betriebsystem wird verwendet (Die Sicherheit ist eingeschränkt)
- Kompliziert in Sache Maintaining und Einrichtung
- Geringe Komptabilität mit Windows
- Wenig Fachpersonal für Wartung

## Vagrant
**Theorie**
Vagrant ist eine freie Ruby-Anwendung zum Erstellen und Verwalten von virtuellen Maschinen. Vagrant ermöglicht einfache Softwareverteilung (englisch Deployment) insbesondere in der Software- und Webentwicklung und dient als Wrapper zwischen Virtualisierungssoftware wie VirtualBox, KVM/QEMU, VMware und Hyper-V und Software-Configuration-Management-Anwendungen beziehungsweise Systemkonfigurationswerkzeugen wie Chef, Saltstack und Puppet.

**Cheatsheet**

**Hands-On**

**Apache Web Server aufsetzten**

**Apache Webserver aufsetzten**

**Nginx Webserver aufsetzten**
Link zum Auftrag: https://github.com/ser-cal/M300-Vagrant-Webserver

## Docker
**Eigenes Imagine erstellen**

**Cheatsheet**

# Quellen
- https://www.modulbaukasten.ch/module/300/4/de-DE?title=Plattform%C3%BCbergreifende-Dienste-in-ein-Netzwerk-integrieren

- - -
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/ch/"><img alt="Creative Commons Lizenzvertrag" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/3.0/ch/88x31.png" /></a><br />Dieses Werk ist lizenziert unter einer <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/ch/">Creative Commons Namensnennung - Nicht-kommerziell - Weitergabe unter gleichen Bedingungen 3.0 Schweiz Lizenz</a>

- - -
