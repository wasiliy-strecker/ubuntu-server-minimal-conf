````markdown
# Ubuntu Server Einrichtung: ws-forms.com

Diese Anleitung beschreibt die vollständige Einrichtung eines Apache Virtual Hosts auf einem Ubuntu System für die Domain **ws-forms.com**.

---

## 1. Verzeichnisstruktur & Berechtigungen

Zuerst legen wir das Projektverzeichnis an.  
Der aktuelle User bleibt Besitzer, die Gruppe wird `www-data`.

```bash
# Verzeichnis erstellen
sudo mkdir -p /var/www/ws-forms

# Gruppe auf www-data setzen (Besitzer bleibt dein User)
sudo chown -R :www-data /var/www/ws-forms

# RECHTE-OPTIMIERUNG:
# Alle Ordner auf 775 setzen (drwxrwxr-x) -> Gruppe darf in Ordner wechseln
sudo find /var/www/ws-forms -type d -exec chmod 775 {} \;

# Alle Dateien auf 664 setzen (-rw-rw-r--) -> Gruppe darf Dateien bearbeiten
sudo find /var/www/ws-forms -type f -exec chmod 664 {} \;

# Setgid-Bit setzen (neue Dateien erben automatisch die Gruppe www-data)
sudo chmod g+s /var/www/ws-forms
````

---

## 2. Apache Virtual Host Konfiguration

Konfigurationsdatei erstellen:

```bash
sudo nano /etc/apache2/sites-available/ws-forms.conf
```

Inhalt der Datei:

```apache
<VirtualHost *:80>
    ServerAdmin test@example.de
    DocumentRoot /var/www/ws-forms/
    ServerName ws-forms.com

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory /var/www/ws-forms/>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

---

## 3. Seite aktivieren & Apache neu laden

```bash
# Seite aktivieren
sudo a2ensite ws-forms.conf

# Syntax der Konfiguration prüfen
sudo apache2ctl configtest

# Apache neu laden
sudo systemctl reload apache2
```

---

## 4. Lokale DNS-Auflösung (Hosts-Datei)

Hosts-Datei öffnen:

```bash
sudo nano /etc/hosts
```

Folgende Zeile hinzufügen:

```plaintext
127.0.0.1   ws-forms.com
```

---

## 5. Nützliche Befehle zur Verwaltung

| Aktion               | Befehl                                    |
| -------------------- | ----------------------------------------- |
| Seite deaktivieren   | `sudo a2dissite ws-forms.conf`            |
| Apache Status prüfen | `sudo systemctl status apache2`           |
| Error Log einsehen   | `sudo tail -f /var/log/apache2/error.log` |

---

```
```
